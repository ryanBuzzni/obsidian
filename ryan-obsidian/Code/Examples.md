
**액션 중심으로 이름 짓기(함수를 구분짓게 되는 필요성 확인이 됨)**
```javascript
Bad
function userData() {}
const data = userData()

Good
function loadUserData() {
const userData = loadUserData()
}
```

**짧은 변수명이나 (아무도 이해못하는)축약어 쓰는 것을 피하기**
```javascript
Bad
allUser.forEach((u) => {
	sendEmail(u);
})

Good
allUser.forEach((user) => {
	sendEmail(user);
})

Bad
function createOrder(order) {
	conat i = IManager()
	if (i.validate(order.items)) { ... }
	
	conat p = PProcessor()
	if (p.validate(order.p_method)) { ... }
	
	...
}

Good
function createOrder(order) {
	conat inventory = InventoryManager()
	if (inventory.validate(order.items)) { ... }
	
	conat payment = PaymentProcessor()
	if (payment.validate(order.payment_method)) { ... }
	
	...
}
```

**Arrowhead anti pattern, ealry return이나 early exit하기**
```javascript
Bad
function doSomething (number) {
	if (typeof number !== 'number') {
		console.error('Invalid value, Expected number')
		return
	} else {
		if (number > 100) {
		
		} else {
			if (number % 2 === 1) {
			
			} else {
			
			}
		}
	}
}

Good
function doSomething (number) {
	if (typeof number !== 'number') {
		console.error('Invalid value, Expected number')
		return
	}
		
	
	if (number > 100) {
		// something
		return
	} 
	
	if (number % 2 === 1) {
		// something
		return
	} 
	
	// something
	
}
```

**잘못된 Error/Exception**
```javascript
// 유저가 탈퇴시에 유저의 관련파일 삭제 함수, 문제점은?
function removeUser(user) {
	try {
		removeUserFiles(user)
	} catch(err) {
		console.warn(err)
	}

	deactivateUser(user)
}
```

**타입에 의한 if 문을 객체로 변형해서 사용**

**boolean형 변수 이름 nagative로 짓지 않기**
```js
BAD
const unPressable = true
const isAllUnableDeliver = true

GOOD
const pressable = false
const isAllDeliver = false
```

**Single Responsibility principle**
_단일책임원칙_ single function -> multi x
**함수**는 오직 하나의 책임을 가져야 한다. (**함수**는 오직 하나의 변경의 이유만을 가져야 한다.)  
같은 이유로 변경될 코드들은 모으고. 다른 이유로 변경될 코드들은 흩어라.
```js
BAD
class cafeOwner {
  constructor(coffeebeans) {
    this.coffeebeans = coffeebeans;
  }
  
  manageShop(time){
  	console.log(`managing coffee shop at ${time}`)
  }

  makeCoffee(coffeebeans) {
    console.log(`making cofffe with ${coffeebeans}`)
  }

  serveCoffee(guest) {
    console.log(`serving coffee to ${guest}`)
  }
}

GOOD
class coffemaker {
  makeCoffee(coffeebeans) {
    console.log(`making cofffe with ${coffeebeans}`)
  }
}

class coffeeServer{
  serveCoffee(guest) {
    console.log(`serving coffee to ${guest}`)
  }
}

class cafeOwner {
  coffee;
  constructor(coffeebeans) {
    this.coffeebeans = coffeebeans;
  }

  manageShop(time){
  	console.log(`managing coffee shop at ${time}`)
  }
}
```

**Open-Closed principle**
_개방폐쇄원칙_ function params 
소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.

```js
BAD
function getMutipledArray(array, option) {
  const result = []
  for (let i = 0; i < array.length; i++) {
    if (option === "doubled") {
      result[i] = array[i] * 2 // 새로운 방식으로 만들기 위해서는 수정이 필요하다.
    }
    if (option === "tripled") {
      result[i] = array[i] * 3 // 옵션으로 분기는 가능하나
    }
    if (option === "half") {
      result[i] = array[i] / 2 // 새로운 기능을 추가하려면 함수 내에서 변경이 되어야 한다.
    }
  }
  return result
}

GOOD
// option을 받는게 아니라 fn을 받아보자.
// 이제 새로운 array를 만든다는 매커니즘은 닫혀있으나 방식에 대해서는 열려있다.
function map(array, fn) {
  const result = []
  for (let i = 0; i < array.length; i++) {
    result[i] = fn(array[i], i, array) // 내부 값을 외부로 전달하고 결과를 받아서 사용한다.
  }
  return result
}

// 얼마든지 새로운 기능을 만들어도 map코드에는 영향이 없다.
const getDoubledArray = (array) => map(array, (x) => x * 2)
const getTripledArray = (array) => map(array, (x) => x * 3)
const getHalfArray = (array) => map(array, (x) => x / 2)

BAD
class LatteMaker {
  coffee = "Latte";
}

class TeaMaker {
  coffee = "Blacktea"
}

class cafeOwner {
  constructor(maker, server) {
    this.maker = maker
  }

  makeCoffee(){
    if(this.maker.coffee==="Latte"){
      brewingLatte()
    }else if(this.maker.coffee==="Tea"){
      brewingBlacktea()
    }
  }
}

GOOD
class LatteMaker {
  coffee = "Latte";
  brewingCoffee(){
    console.log(`making coffee with Milk`)
  }
}

class BlackteaMaker {
  coffee = "Blacktea"
  brewingCoffee(){
    console.log(`making coffee without Milk`)
  }
}

class cafeOwner {
  constructor(maker, server) {
    this.maker = maker
  }

  makeCoffee(){
    this.maker.brewingCoffee()
  }
}
```

**useImperativeHandle 공유**

**DRY - Don't Repeat Yourself**
_같은 코드를 중복해서 작성하지 않는다._ 시스템이 소규모일때는 복잡도가 크기 않기 때문에 프로그램을 이해하기가 수월한 반면 시스템이 커지고 개념도 많아지면 복잡도가 기하급수적으로 높아지게 된다. 이런 시스템에서는 복잡도를 최대한 줄여야 개발 및 나중에 유지보수비용이 절감이 된다. 

**KISS - Keep it simple, stupid**
_KISS는 “Keep it small and simple.”, “Keep it short and simple.”, 또는 “Keep it simple, stupid.”를 나타내는데, 소프트웨어 디자인을 간단하고 단순하게 하는 것이다. "다른사람에게 설명할 수 없다면 당신은 제대로 이해한 게 아닙니다"._ 즉, 큰 프로젝트를 단순하게 디자인 하지 못하고 복잡하게만 구현을 한다는 것은 프로젝트를 제대로 이해하지 못했다는 증거이다. 프로젝트가 진행되기 전에 최대한 기반 배경과 추진되는 목적자체를 이해하고 어떻게 구현을 단순화하고 알기쉽게 설계할 수 있을지 회의를 해서 개선해야한다.

**YAGNI - You Ain't Gonna Need it**
_정말 필요할 때까지 그 기능을 만들지 말라._
현재 필요하지 않지만 향후에 필요가능성을 대비해서 미리 함수나 코드를 작성하지말고 지금 필요한 기능만 추가해야합니다. 미리 필요없는 기능을 추가해두면 코드 자체가 길어지기 때문에 분석자체가 더 어려워지며 또한 버그가 발생 할 가능성이 커집니다. 

