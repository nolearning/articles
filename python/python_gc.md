# Python GC

## Python Object
```c
/* Define pointers to support a doubly-linked list of all live heap objects. */
#define _PyObject_HEAD_EXTRA            \
    struct _object *_ob_next;           \
    struct _object *_ob_prev;
    
typedef struct _object {
    _PyObject_HEAD_EXTRA
    Py_ssize_t ob_refcnt;
    struct _typeobject *ob_type;
} PyObject;


```
* [cpython/Include/object.h](https://github.com/python/cpython/blob/master/Include/object.h)
* [Everything’s an object](https://pythoninternal.wordpress.com/2014/08/11/everythings-an-object/)

## Python GC
```c
/* Get an object's GC head */
#define AS_GC(o) ((PyGC_Head *)(o)-1)

/* Get the object given the GC head */
#define FROM_GC(g) ((PyObject *)(((PyGC_Head *)g)+1))
```

```c
/* A traversal callback for subtract_refs. */
static int
visit_decref(PyObject *op, void *data)
{
    assert(op != NULL);
    if (PyObject_IS_GC(op)) {
        PyGC_Head *gc = AS_GC(op);
        /* We're only interested in gc_refs for objects in the
         * generation being collected, which can be recognized
         * because only they have positive gc_refs.
         */
        if (gc_is_collecting(gc)) {
            gc_decref(gc);
        }
    }
    return 0;
}

/* Subtract internal references from gc_refs.  After this, gc_refs is >= 0
 * for all objects in containers, and is GC_REACHABLE for all tracked gc
 * objects not in containers.  The ones with gc_refs > 0 are directly
 * reachable from outside containers, and so can't be collected.
 */
static void
subtract_refs(PyGC_Head *containers)
{
    traverseproc traverse;
    PyGC_Head *gc = GC_NEXT(containers);
    for (; gc != containers; gc = GC_NEXT(gc)) {
        traverse = Py_TYPE(FROM_GC(gc))->tp_traverse;
        (void) traverse(FROM_GC(gc),
                       (visitproc)visit_decref,
                       NULL);
    }
}
```
```
static inline void
gc_reset_refs(PyGC_Head *g, Py_ssize_t refs)
{
    g->_gc_prev = (g->_gc_prev & _PyGC_PREV_MASK_FINALIZED)
        | PREV_MASK_COLLECTING
        | ((uintptr_t)(refs) << _PyGC_PREV_SHIFT);
}

/* Set all gc_refs = ob_refcnt.  After this, gc_refs is > 0 and
 * PREV_MASK_COLLECTING bit is set for all objects in containers.
 */
static void
update_refs(PyGC_Head *containers)
{
    PyGC_Head *gc = GC_NEXT(containers);
    for (; gc != containers; gc = GC_NEXT(gc)) {
        gc_reset_refs(gc, Py_REFCNT(FROM_GC(gc)));
        /* Python's cyclic gc should never see an incoming refcount
         * of 0:  if something decref'ed to 0, it should have been
         * deallocated immediately at that time.
         * Possible cause (if the assert triggers):  a tp_dealloc
         * routine left a gc-aware object tracked during its teardown
         * phase, and did something-- or allowed something to happen --
         * that called back into Python.  gc can trigger then, and may
         * see the still-tracked dying object.  Before this assert
         * was added, such mistakes went on to allow gc to try to
         * delete the object again.  In a debug build, that caused
         * a mysterious segfault, when _Py_ForgetReference tried
         * to remove the object from the doubly-linked list of all
         * objects a second time.  In a release build, an actual
         * double deallocation occurred, which leads to corruption
         * of the allocator's internal bookkeeping pointers.  That's
         * so serious that maybe this should be a release-build
         * check instead of an assert?
         */
        _PyObject_ASSERT(FROM_GC(gc), gc_get_refs(gc) != 0);
    }
}
```
```
/* This is the main function.  Read this to understand how the
 * collection process works. */
static Py_ssize_t
collect(int generation, Py_ssize_t *n_collected, Py_ssize_t *n_uncollectable,
        int nofail)
{
...
/* Using ob_refcnt and gc_refs, calculate which objects in the
     * container set are reachable from outside the set (i.e., have a
     * refcount greater than 0 when all the references within the
     * set are taken into account).
     */
    update_refs(young);  // gc_prev is used for gc_refs
    subtract_refs(young);
...
}
```

* [cpython/Modules/gcmodule.c](https://github.com/python/cpython/blob/master/Modules/gcmodule.c)

### gc collect algorithm
1. get generation num
2. get generation gc container
3. update gc_refs of all objects in containers
4. visit all object in container, and for each gc collectable object, call tp_travers of the object to decrease the gc_refs of the objects referneced by the object.

### example of user defined type(class)
```c
static PyObject *
type_new(PyTypeObject *metatype, PyObject *args, PyObject *kwds)
{
...
/* Allocate the type object */
  type = (PyTypeObject *)metatype->tp_alloc(metatype, nslots);
...
  /* Always override allocation strategy to use regular heap */
  type->tp_alloc = PyType_GenericAlloc;
  if (type->tp_flags & Py_TPFLAGS_HAVE_GC) {
      type->tp_free = PyObject_GC_Del;
      type->tp_traverse = subtype_traverse;
      type->tp_clear = subtype_clear;
  }
  else
      type->tp_free = PyObject_Del;
...
}

static int
subtype_traverse(PyObject *self, visitproc visit, void *arg)
{
...
}
```

* [cpython/Objects/typeobject.c](https://github.com/python/cpython/blob/master/Objects/typeobject.c)
* [Python 源码阅读——垃圾回收机制](http://python.jobbole.com/83548/)
* [The Garbage Collector](https://pythoninternal.wordpress.com/2014/08/04/the-garbage-collector/)
