# Physics

## Momentum

## Kinetic energy

## Elastic collision
Based on `law of momentum conservation` and `law of conservation of kinetic energy`, we have
```
m1 * v1 + m2 * v2 = m1 * u1 + m2 * u2
m1 * v1 * v1 / 2 + m2 * v2 * v2 / 2 = m1 * u1 * u1 / 2 + m2 * u2 * u2 / 2
```
* solution 1 - directly solve
```
u2 = (m1 * v1 + m2 * v2 - m1 * u1) / m2
// (m1 + m2) * m1 * u1 * u1 - 2 * (m1 * v1 + m2 * v2) * m1 * u1 + (m1 - m2) * m1 * v1 * v1 + 2 * m1 * m2 * v1 * v2 = 0
(m1 + m2) * u1 * u1 - 2 * (m1 * v1 + m2 * v2) * u1 + ((m1 - m2) * v1 + 2 * m2 * v2) * v1 = 0
// (m1 + m2) * v1 + (m1 - m2) * v1 + 2 * m2 * v2 = 2 * (m1 * v1 + m2 * v2)
u1 = ((m1 - m2) * v1 + 2 * m2 * v2) / (m1 + m2)
u2 = (2 * m1 * v1 + (m2 - m1) * v2) / (m1 + m2)
```

* solution 2 - derivation
```
m1 * (v1 - u1) = m2 * (u2 - v2)
m1 * (v1 - u1) * (v1 + u1) = m2 * (u2 - v2) * (u2 + v2)
// so
v1 + u1 = v2 + u2 => v1 - v2 = u2 - u1
// from m1 * v1 + m2 * v2 = m1 * u1 + m2 * u2
u1 = ((m1 - m2) * v1 + 2 * m2 * v2) / (m1 + m2)
u2 = (2 * m1 * v1 + (m2 - m1) * v2) / (m1 + m2)
```

* [Elastic collision](https://en.wikipedia.org/wiki/Elastic_collision)
