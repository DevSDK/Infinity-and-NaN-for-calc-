# Allow calc function infinity and NaN
Author: [Seokho Song](https://github.com/devsdk)(0xdevssh@gmail.com)

## Summary

 According to the CSS specification [CSS Values and Units Module Level 4](https://drafts.csswg.org/css-values/#calc-type-checking), CSS calc() math function should allow infinity and NaN values by 'infinity', '-infinity', 'NaN' keywords or expressions that could be evaluated into infinity or NaN such as 'calc(1/0)'.

## Infinity and NaN operations

The infinity and NaN will be evaluated by [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754). 

The operations: 
| Operation                                     | Result    |
|-----------------------------------------------|-----------|
| n ÷ ±infinity                                 | 0         |
| ±infinity × ±infinity                         | ±infinity |
| ±n ÷ ±0  (n != 0)                             | ±infinity |
| ±n × ±infinity (n is finite, n != 0)          | ±infinity |
| infinity + infinity<br>infinity – -infinity   | +infinity |
| -infinity – infinity<br>  -infinity + –infinity | -infinity |
| ±0 ÷ ±0                                       | NaN       |
| ±infinity ÷ ±infinity                         | NaN       |
| ±infinity × 0                                 | NaN       |

## Implementation aspect

 All of the values evaluated in infinity or NaN should be clamped to ideal finite values before consuming to components.

## Examples
 Below infinity and NaN could be evaluated by the expression such as Zero Division. The result of using the expression and keywords is identical.

1. **\<length> and \<length-percentage>**
    ```CSS
    div {
        width:calc(infinity*1px);
        height:10px;
        background-color:green;
    }
    ```
    
    This case div's computed width will be a maximum value(≈3.35544e+07) of computed style.
     
     In case of NaN:
     ``` CSS
        width:calc(NaN*1px);
     ```
     The value will be a maximum value(≈3.35544e+07) of computed style to indicate infinity.

2. **\<time>**
   ```CSS
    div {
        animation-duration: calc(infinity*1s);
        animation-name: SomeAnimation;
    }
   ```
    This case div's computed duration will be maximum value(≈3.35544e+07) of computed style.
    
     
     In case of NaN:
     ``` CSS
        animation-duration:calc(NaN*1s);
     ```
     The value will be a maximum value(≈3.35544e+07) of computed style to indicate infinity.

3. **\<angle>** : Under discusssion
   ```CSS
    div {
        transform: rotate(calc(infinity*1deg));
    }
   ```
    ```CSS
    div {
        transform: rotate(calc(NaN*1deg));
    }
   ```
   Work In Progress.

4. **Interpolation** : WIP
    
    Strictly followed the spec, It's interpolated by enormous clamped values. However, this may need discussion. 

### See Also

**Design Doc:** [doc](https://docs.google.com/document/d/1kksm8aa5HpCph5NmJEwrCrj2e3p85RORQCr7OSsWAOs/edit?usp=sharing)

**Issue:** [1133390](https://crbug.com/1133390)
