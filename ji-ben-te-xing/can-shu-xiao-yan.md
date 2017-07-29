# 参数校验

@param 和 @var 支持嵌套校验

##语法 

```@v <rule>[:param]|<rule2>...``` 如
```@v optional|min:0|max:10```

## 支持的规则

* required - Required field
* equals - Field must match another field (email/password confirmation)
* different - Field must be different than another field
* accepted - Checkbox or Radio must be accepted (yes, on, 1, true)
* numeric - Must be numeric
* integer - Must be integer number
* boolean - Must be boolean
* array - Must be array
* length - String must be certain length
* lengthBetween - String must be between given lengths
* lengthMin - String must be greater than given length
* lengthMax - String must be less than given length
* min - Minimum
* max - Maximum
* in - Performs in_array check on given array values
* notIn - Negation of in rule (not in array of values)
* ip - Valid IP address
* email - Valid email address
* url - Valid URL
* urlActive - Valid URL with active DNS record
* alpha - Alphabetic characters only
* alphaNum - Alphabetic and numeric characters only
* slug - URL slug characters (a-z, 0-9, -, _)
* regex - Field matches given regex pattern
* date - Field is a valid date
* dateFormat - Field is a valid date in the given format
* dateBefore - Field is a valid date and is before the given date
* dateAfter - Field is a valid date and is after the given date
* contains - Field is a string and contains the given string
* creditCard - Field is a valid credit card number
* instanceOf - Field contains an instance of the given class
* optional - Value does not need to be included in data array. If it is however, it must pass validation.