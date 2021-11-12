

# initState

## validateProp

> 判断传入的属性是否满足validate条件

```javascript
// src/core/util/props.js

// key: 遍历propsOptions 时拿到的每个属性名
// propOptions: 当前实例规范化的props选项
// propsData: 父组件传入的props数据
// vm: 当前实例
export function validateProp(key, propOptions, propsData, vm) {
    const prop = propsOptions[key];
    // 父组件是否传入该属性
    const absent = !hasOwn(propsData, key);
    // 父组件传过来的真实值
    let value = propsData[key];
    // 获取boolean类型在prop.type中的优先级（index）
    const booleanIndex = getTypeIndex(Boolean, prop.type);
    
    // 存在booleanIndex
    if(booleanIndex > -1) {
        // 没有传入值并且没有默认属性
        if(absent && !hasOwn(prop, 'defualt')) {
            value = false;
        } else if(value === '' || value = hyphenate(key)) {
            // 获取string类型数据在prop.type中的优先级
            const stringIndex = getTypeIndex(String, prop.type);
            // prop.type中不存在string类型数据或者boolean类型优先级大于string类型
            if(stringInex < 0 || booleanIndex < stringIndex) {
                value = true;
            }
		}
    }
    
    
    if(value === undefined) {
        // 获取prop的默认值
        value = getPropDefaultValue(vm, prop, key);
        
        // 获取当前是否需要观察的布尔值
        const preShouldObserve = shouldObserve;
       	// 开启observe状态
        toggleObserving(true);
        observe(value);
        // 回复之前是否需要观察的状态
        toggleObserving(preShouldObserve);
    } 
    if(process.env.NODE_ENV !== 'production') {
        assertProp(prop, key, value, vm, absent);
	}
    
    return value;
}
```

### getPropDefaultValue

> 获取默认值函数

```javascript
function getPropDefaultValue(vm, prop, key) {
    if(!hasOwn(prop, 'default')) {
        return undefined;
	}
    
    cosnt def = prop.default;
    
    if(process.env.NODE_ENV !== 'production' && isObject(def)) {
        warn(
        ),
        vm
	}
    
    if(vm && vm.$options.propsData &&
      vm.$options.propsData[key] === undefined &&
      vm._props[key] !== undefined) {
        return vm._props[key];
    }
    
    return typeof def === 'function' && getType(prop.type) !== 'Function'
    		? def.call(vm)
    		: def
}
```

