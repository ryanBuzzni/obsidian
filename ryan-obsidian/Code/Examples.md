
[액션 중심으로 이름 짓기(함수를 구분짓게 되는 필요성 확인이 됨)]
```javascript
Bad
function userData() {}
const data = userData()

Good
function loadUserData() {
const userData = loadUserData()
}
```

[짧은 변수명이나 (아무도 이해못하는)축약어 쓰는 것을 피하기]
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

[Arrowhead anti pattern, ealry return이나 early exit하기]
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

[잘못된 Error/Exception]
```javascript
// 유저가 탈퇴시에 유저의 관련파일 삭제 함수
function removeUser(user) {
	try {
		removeUserFiles(user)
	} catch(err) {
		console.warn(err)
	}

	deactivateUser(user)
}
```