# Question
三角形三内角有如下大小关系， 角A:角B:角C=1:2:4。
证:1/AB+1/AC=1/BC
           C
           *
        *     *
    *            *
*   *  *   *   *  *
A                  B

## solution 1
用正弦定理加上倍角公式

AB/sin4A = AC / sin2A = BC / sinA 

1/AB + 1/AC = 1/BC * (sinA/sin2A + sinA/sin4A) 

sinA/sin2A + sinA/sin4A = 2sinAcos2A/sin4A + sinA/sin4A = (2sinA * (1-2sinA * sinA) + sinA)/sin4A = sin3A / sin4A

3A + 4A = 180, sin3A / sin4A = 1

sinA/sin2A + sinA/sin4A = 1

* [倍角公式](https://baike.baidu.com/item/%E5%80%8D%E8%A7%92%E5%85%AC%E5%BC%8F)
* [正弦定理](https://baike.baidu.com/item/%E6%AD%A3%E5%BC%A6%E5%AE%9A%E7%90%86)
## solution 2

> thinking
> 角B做角平分线，交AC于点P，由角B = 2 * 角A = 2 * 角CBP = 2 * 角ABP，  
> 得三角形BCP相似于三角形BAC，得BP = AP， BC/AC = CP /BC = BP/AB = AP/AB  
> CP=BC*BC/AC， AP= BC*AB / AC， AC*AC - BC*BC = BC * AB  
> 同理， 得 AB*AB - AC*AC = AC*BC， 推导至AB*AB-BC*BC = AC*BC + BC*AB 无法继续

令P为BC延长线上一点，角PAC = 角CAB， 可得角PAB = 角CBA， 角APB = 角ACP = 3倍角CAB， PA = PB = AC  
另由角平分线性质， PC/BC = PA / AB， （AC-BC）/ BC = AC / AB， AC * BC + BC * AB = AC*AB， 两边同除AB*BC*AC，得1/AB + 1/AC = 1/BC
