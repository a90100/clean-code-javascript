# 無瑕的程式碼 JavaScript
> 原作： [https://github.com/ryanmcdermott/clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)<br>
> 原作者： [https://github.com/ryanmcdermott](https://github.com/ryanmcdermott) <br>
> 譯者： [https://github.com/trylovetom](https://github.com/trylovetom) 

## 目錄（Table of Contents）
1. [介紹（Introduction）](#介紹Introduction)
2. [變數（Variables）](#變數Variables)
3. [函數（Functions）](#函數Functions)
4. [物件（Objects）與資料結構（Data Structures）](#物件Objects與資料結構Data-Structures)
5. [類別（Classes）](#類別Classes)
6. [物件導向基本原則（SOLID）](#物件導向基本原則SOLID)
7. [測試（Testing）](#測試Testing)
8. [並發（Concurrency）](#並發Concurrency)
9. [錯誤處理（Error Handling）](#錯誤處理Error-Handling)
10. [格式化（Formatting）](#格式化Formatting)
11. [註解（Comments）](#註解Comments)
12. [翻譯（Translation）](#翻譯Translation)

## 介紹（Introduction）
![透過計算閱讀程式碼時的咒罵次數，來評估軟體品質](http://www.osnews.com/images/comics/wtfm.jpg)<br>透過計算閱讀程式碼時的咒罵次數，來評估軟體品質<br><br>
文章作者根據 Robert C. Martin 的[《無暇的程式碼》](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)，撰寫一份適用於 JavaScript 的原則。本文不是風格指南（Style Guide），而是教導你撰寫出[可閱讀、可重複使用與可重構](https://github.com/ryanmcdermott/3rs-of-software-architecture)的 JS 程式碼。

注意！你不必嚴格遵守每一項原則，有些甚至不被大眾所認同。雖然這只是份指南，卻是來自《無暇的程式碼》作者的多年結晶。

軟體工程只發展了五十年，仍然有很多地方值得去探討。當軟體與建築一樣古老時，也許會有一些墨守成規的原則。但現在，先讓這份指南當試金石，作為你和團隊的 JS 程式碼標準。

還有一件事情：知道這些原則，並不會立刻讓你成為出色的開發者，長期奉行它們，不代表你能高枕無憂不再犯錯。但是，千里之行，始於足下，時常與志同道合們進行討論（Code Review），改善不完備之處。不要因為自己寫出來的程式碼很糟糕而害怕分享，而是要畏懼自己居然寫出了這樣的程式碼！

**譯者序**
> 獻給[傲嬌文創](https://alljoint.tw/)的所有工程師夥伴，與熱愛 JavaScript 的各位。

《無暇的程式碼》是一本好書，是不可否認這個事實。我熱愛著 JS，當找到這份 JS 版本的後，我無比開心立刻著手翻譯，因此有了這份《無暇的程式碼 JavaScript》。對於 *Clean Code* 的書名採用博碩文化的翻譯，也是對原版譯者的尊敬。本翻譯中會穿插一些我對文章中註解，也請讀者原諒我的叨擾。另外專業術語的翻譯有可能會有出入，我會標示出英文原文，避免讀者誤解。如有翻譯或是理解上的錯誤，煩請聯絡我，謝謝。（聯絡方式在上方，我的 GitHub 中。）

## 變數（Variables）
### 使用具有意義且可閱讀的名稱
**糟糕的：**
```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**適當的：**
```javascript
const currentDate = moment().format('YYYY/MM/DD');
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 相同類型的變數使用相同的名稱
**糟糕的：**
```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**適當的：**
```javascript
getUser();
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 使用可搜尋的名稱
使用易於閱讀與搜尋的名稱非常重要，因為我們要閱讀的程式碼遠比自己寫得多。使用沒有意義的名稱，會導致程式碼難以理解，對後續閱讀者是個糟糕的體驗。另外使用以下工具，可以協助你找出未命名的常數：
  * [buddy.js](https://github.com/danielstjules/buddy.js)
  * [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)

**糟糕的：**
```javascript
// 86400000 代表什麼意義？
setTimeout(blastOff, 86400000);
```

**適當的：**
```javascript
// 宣告（Declare）一個有意義的常數（constants）
const MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 使用可解釋的變數
**糟糕的：**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**適當的：**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**譯者附註**

`address.match(cityZipCodeRegex)` 取出了字串中的 city 與 zipCode 並以陣列（Array）的方式輸出。在糟糕的範例中，你不會知道哪個是 city，哪個是 zipCode。在適當的範例中，則清楚地解釋了。

**[⬆ 回到目錄](#目錄table-of-contents)**

### 避免心理作用（Mental Mapping）
清晰（Explicit）的表達會比隱藏（Implicit）更好。

**糟糕的：**
```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // 等等，這 `l` 是…？
  dispatch(l);
});
```

**適當的：**
```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

**譯者附註**

在糟糕的範例中，程式碼的作者認為從 `locations` 取出的都是地址，所以選用縮減後的 `l`  作為名稱。不過這只有作者自己這麼認為，其他人可不一定知道。避免「我認為」、「我以為」、「我覺得」，這樣的心理作用。

**[⬆ 回到目錄](#目錄table-of-contents)**

### 避免使用不必要的描述（Context）
如果你的類別與物件名稱是有關聯意義的，就不用在內部變數上再次重複。

**糟糕的：**
```javascript
const Car = {
  carMake: 'Honda',
  carModel: 'Accord',
  carColor: 'Blue'
};

function paintCar(car, color) {
  car.carColor = color;
}
```

**適當的：**
```javascript
const Car = {
  make: 'Honda',
  model: 'Accord',
  color: 'Blue'
};

function paintCar(color) {
  car.color = color;
}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 使用預設參數（Parameter）代替條件判斷（Conditionals）
使用預設參數較整潔，但請注意，當參數為 `undefined` 時才會作用，其他種類的虛值（Falsy）不會，像是 `''`、`false`、`null`、`0`、`NaN` 等。

**糟糕的：**
```javascript
function createMicrobrewery(name) {
  const breweryName = name || 'Hipster Brew Co.';
  // ...
}
```

**適當的：**
```javascript
function createMicrobrewery(name = 'Hipster Brew Co.') {
  // ...
}
```

**譯者附註**

預設參數非常好用，可以結合工廠模式做出很多應用。另外建議統一使用 `undefined` 代替 `null` 當作空值的回傳值。

此處原文為 **Arguments**，但是函數的參數定義應該為 **Parameter**，而呼叫函數時傳遞的引數才是 **Arguments**。

**[⬆ 回到目錄](#目錄table-of-contents)**

## 函數（Functions）
### 參數（Parameter） (少於 2 個較佳)
限制函數的參數數量非常重要，因為能讓你更容易地測試。過多的參數代表著過多的組合，會導致你不得不編寫出大量測試。

一個至二個是最理想的，盡可能避免大於三個以上。如果你有超過兩個以上的參數，代表你的函數做太多事情。如果無法避免時，可以有效地使用物件替代大量的參數。

為了讓你可以清晰地表達，預期使用哪些物件的屬性（Properties），可以使用 ES2015/ES6 提供的解構（Destructuring）語法。使用這種語法有以下優點：
  1. 函數需要物件的哪些屬性，可以像參數一樣清晰地表達。
  2. 解構語法會複製來自物件的原始型態（Primitive），這能幫助你避免副作用（Side Effect）。注意！巢狀物件與陣列，並不會被解構語法複製。
  3. 使用解構語法能讓物件屬性被程式碼檢查器（Linter）作用，提醒你哪些屬性未被使用到。

**糟糕的：**
```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
```

**適當的：**
```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

**譯者附註**

這種方法非常適合用於工廠模式，結合上一章的預設參數，譯者推薦的使用方式如下：
```javascript
function createMenu({
  title = 'Default Title', // 傳遞的物件不齊全，使用預設屬性
  body = '',
  buttonText = 'My Button',
  cancellable = true
} = {}) { // 如未傳遞任何參數使用預設空物件，使用 `= {}` 可避免 TypeError: Cannot destructure property `...` of 'undefined' or 'null'.
  return {
    title,
    body,
    buttonText,
    cancellable
  }
}

const myMenu = createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

想了解更多工廠模式與 ES6 的結合，可參考連結文章：[JavaScript Factory Functions with ES6+](https://medium.com/javascript-scene/javascript-factory-functions-with-es6-4d224591a8b1)。

**[⬆ 回到目錄](#目錄table-of-contents)**

### 一個函數只做一件事情（單一性）
這是個非常重要的原則，當你的函數做超過一件事情時，它會更難以被撰寫、測試與理解。當你隔離（Isolate）你的函數到只做一件事情時，它能更容易地被重構（Refactor）與清晰地閱讀。如果嚴格遵守此項原則，你將會領先許多開發者。

**糟糕的：**
```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**適當的：**
```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 函數名稱應該說明它做的內容
**糟糕的：**
```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// 難以從函數名稱看出到底加入了什麼
addToDate(date, 1);
```

**適當的：**
```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**譯者附註**

建議函數命名以動詞開頭，像是 `doSomething()`、`setupUserProfile()`。

**[⬆ 回到目錄](#目錄table-of-contents)**

### 函數應該只做一層抽象（Abstraction）
當你的函數需要的抽象多於一層時，代表你的函數做太多事情了。將其分解以利重用與測試。

**糟糕的：**
```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(node => {
    // parse...
  });
}
```

**適當的：**
```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach(node => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(' ');
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach(token => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

**譯者附註**

這個原則與前面提到的「一個函數只做一件事情（單一性）」概念相似。

**[⬆ 回到目錄](#目錄table-of-contents)**

### 移除重複的（Duplicate）程式碼
絕對避免重複的程式碼，重複的程式碼代表著更動邏輯時，需要同時修改多處。

想像一下你經營著一家餐廳，需要持續追蹤存貨：番茄、洋蔥、大蒜與各種香料等。如果你有多份紀錄表，當使用蕃茄做完一道料理時，需要更新多份記錄表，可能會忘記更新其中一份。如果你只有一份記錄表，就不會有此問題！

通常重複的程式碼，是因為有兩個稍微不同的東西。它們之間絕大部分相同，但些微不同之處，迫使你使用多個函數處理相似的事情。如果出現重複的程式碼，可以使用函數、模組或是類別來抽象化處理。

正確的抽象化是非常關鍵的，這也是為什麼你應該遵循[類別（Classes）](#類別Classes)章節中，物件導向基本原則 SOLID 的原因。請小心，較差的抽象化會比重複的程式碼更糟！這麼說吧，如果你有把握做出好的抽象化，盡情放手去做。別讓程式碼出現重複的地方，不然你會需要修改更多的程式碼。

**糟糕的：**
```javascript
function showDeveloperList(developers) {
  developers.forEach(developer => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**適當的：**
```javascript
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience
    };

    switch (employee.type) {
      case 'manager':
        data.portfolio = employee.getMBAProjects();
        break;
      case 'developer':
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```

**譯者附註**

剛讀完這個原則時，我非常遵守，但是個人龜毛的個性，造成了不少麻煩，我會在開發時不斷的思考是否會出現了重複的程式碼，甚至考慮到了之後的重用性。代價就是過度設計（Over Engineering）造成功能開發窒礙難行，設計了良好架構，但實際上並未被使用到。最後總結了一個建議作為附加原則：一開始撰寫程式碼先以功能開發優先，當你發現有兩個以上的地方重複時，再來考慮要不要重構。

**[⬆ 回到目錄](#目錄table-of-contents)**

### 使用 `Object.assign` 設定 `Object` 的預設值

**糟糕的：**
```javascript
const menuConfig = {
  title: null,
  body: 'Bar',
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**適當的：**
```javascript
const menuConfig = {
  title: 'Order',
  // 使用者漏掉了 'body'
  buttonText: 'Send',
  cancellable: true
};

function createMenu(config) {
  let finalConfig = Object.assign(
    {
      title: 'Foo',
      body: 'Bar',
      buttonText: 'Baz',
      cancellable: true
    },
    config
  );
  return finalConfig;
  // config 現在等同於： {title: 'Order', body: 'Bar', buttonText: 'Send', cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 不要使用旗標（Flag）作為參數
當你的函數使用了旗標當作參數時，代表函數做不只一件事情，依照不同旗標路徑切分你的函數。

**糟糕的：**
```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**適當的：**
```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 避免副作用（Side Effects）
當函數作用在除了回傳值外的地方，像是讀寫文件、修改全域變數或是將你的錢轉帳到其他人帳戶，則稱為副作用。

程式在某些情況下是需要副作用的，像是上面所提到的例子。這時你應該將這些功能集中在一起，不要同時有多個函數或是類別同時操作資源，應該只用一個服務（Service）完成這些事情。

常見的問題像是：
  * 在沒有任何架構下，同時多個物件中分享共有狀態。
  * 可變的狀態，且可以被任何人寫入
  * 副作用發生的地方沒有被集中。

如果你能避免這些問題，你會比大多數的工程師快樂。

**糟糕的：**
```javascript
// 全域變數被以下函數使用
// 假如有其他的函數使用了這個名稱，現在他變成了陣列，將會被破壞而出錯。
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**適當的：**
```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

const name = 'Ryan McDermott';
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 避免副作用（Side Effects）第二部分
在 JavaScript 中，原始資料類型傳遞數值（Value），物件/陣列傳遞參照（Reference）。在本案例中，你的函數改變了購物車清單 `cart` 中的陣列，像是你增加了一個商品，其他使用購物車清單的函數將會被影響。這做法有好有壞，讓我解釋一下問題所在：

使用者按下付款按鈕後，將會呼叫 `purchase` 函數，產生一個網路請求傳送購物車清單陣列到伺服器。因為較差的網路連線，必須多嘗試幾次。此時使用者不小心又按下加入購物車按鈕，因為是參考的關係，請求將會送出新加入的商品。

較好的解決辦法是 `addItemToCart` 函數，執行前複製新的一份購物車清單，修改複製的資料後再回傳。這能確保其他的函數的購物車清單沒有任何機會被被參考所影響。

使用這方法前，有兩個警告要告知：
1. 當採用這種做法後，你會發現，需要修改輸入物件的情況非常少。大多數的程式碼可以在沒有副作用的情況下重構！

2. 複製大型物件，需要花費高昂的效能。幸好，我們有好的[函數庫](https://immutable-js.github.io/immutable-js/)，可以提升複製物件與陣列的速度與減少記憶體使用。

**糟糕的：**
```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**適當的：**
```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**譯者附註**

譯者這邊另外再提供一個案例，函數 `checkIs18Age` 用來檢查是否成年。第一個寫法中，引用了全域（global）變數 `minimum`，這功能看起來能正常運作沒問題，但是今天如果有其他函數 `setMinAge` 修改了全域變數 `minimum`，函數 `checkIs18Age` 將會因為副作用的關係變的無法預期，甚至失去它的作用。

較好的寫法是，採用純函數（pure function），使用一個可預期的變數，來避免副作用影響。

```javascript
let minimum = 18

// impure with side effect
const checkIs18Age = age => age >= minimum
const setMinAge = age => minimum = age

// pure
const checkIs18Age = age => {
  let minimum = 18
  return age >= minimum
}
```

另外使用解構的方式複製資料，只會複製第一層而已。

```javascript
// 巢狀解構只能複製第一層
const obj1 = { subObj: { message: 'Hey' } }
const obj2 = { ...obj1 }

obj2.subObj.message = 'Yo'
console.log(obj1.subObj.message) // 'Yo'
```

如果要複製巢狀結構，你需要深度複製。可以使用一些函數庫 [loadsh](https://lodash.com/) 的 `_.CloneDeep` 或是 [ramda](https://ramdajs.com/) 的 `clone`。或是使用 `JSON.parse(JSON.stringify(object))` 來實現。不過使用 JSON 的話，會失去 Function 的複製。

```javascript
// 使用 JSON 來深度複製
const obj1 = { subObj: { message: 'Hey' } }
const obj2 = JSON.parse(JSON.stringify(obj1))

obj2.subObj.message = 'Yo'
console.log(obj1.subObj.message) // 'Hey'
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 別寫全域函數（Global Function）
在 JavaScript 中弄髒全域是個不好的做法，因為你可能會影響到其他函數庫或是 API。舉個例子，如果妳想要在 JavaScript 的原生陣列方法，擴展  `diff` 方法，用 B 陣列來去除 A 陣列中的元素（Element）。常見做法你可能會在  `Array.prototype` 中增加一個全新的函數，如果其他函數庫也有自己的 `diff` 實現的話將會互相影響。這就是為什麼我們需要使用 ES2015/ES6 的類別，來擴展的原因。

**糟糕的：**
```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**適當的：**
```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 偏好使用函數式程式（Functional Programming）設計代替命令式程式設計（Imperative Programming）
JavaScript 不是像 Haskell 一樣的函數式語言，但它具有類似特性。函數式程式設計更加乾淨且容易被測試。當你在寫程式時，盡量選擇此設計方式。

**糟糕的：**
```javascript
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  },
  {
    name: 'Suzie Q',
    linesOfCode: 1500
  },
  {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  },
  {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**適當的：**
```javascript
const programmerOutput = [
  {
    name: 'Uncle Bobby',
    linesOfCode: 500
  },
  {
    name: 'Suzie Q',
    linesOfCode: 1500
  },
  {
    name: 'Jimmy Gosling',
    linesOfCode: 150
  },
  {
    name: 'Gracie Hopper',
    linesOfCode: 1000
  }
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 封裝狀態（Encapsulate Conditionals）

**糟糕的：**
```javascript
if (fsm.state === 'fetching' && isEmpty(listNode)) {
  // ...
}
```

**適當的：**
```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === 'fetching' && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 避免負面狀態（Negative Conditionals）

**糟糕的：**
```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**適當的：**
```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 避免狀態
當你第一次聽到時，這聽起來是不可能的任務。大部分的人會說：「怎麼可能不使用 `if` 語法？」事實上你可以使用多態性（Polymorphism） 達到相同的效果。第二個問題來了，「為什麼我們需要這樣做呢？」依據前面概念，為了保持程式碼的乾淨，當類別或是函數出現 `if` 語法，代表你的函數做了超過一件事情。記住，一個函數只做一件事情！

**糟糕的：**
```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case '777':
        return this.getMaxAltitude() - this.getPassengerCount();
      case 'Air Force One':
        return this.getMaxAltitude();
      case 'Cessna':
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**適當的：**
```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
 // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 避免型別（Type）檢查：第一部分
JavaScript 為弱型別語言，代表函數應能處理任何型別的引數（argument）。有時這會帶給你一些麻煩，讓你需要做型別檢查。有很多方法可以避免這種問題發生，第一種就是統一所有的 API。

**糟糕的：**
```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location('texas'));
  }
}
```

**適當的：**
```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location('texas'));
}
```

**譯者附註**

此範例統一了所有的車輛移動的參數、方法與實作，所以不再需要區分不同的類別的車輛呼叫不同的方法。

**[⬆ 回到目錄](#目錄table-of-contents)**

### 避免型別檢查：第二部分
假設你需要型別檢查原始數值，像是字串與整數且你無法使用多態性處理，考慮使用 TypeScript 吧。他是提供標準 JavaScript 靜態類型的的最佳選擇。手動型別檢查需要很多額外處理，你得到的是虛假的型別安全，且失去的可讀性。保持你的 JavaScript 程式碼的整潔、寫好測試與足夠的程式碼審查（Code Review）。如果加上使用 TypeScript 會是更好的選擇。

**糟糕的：**
```javascript
function combine(val1, val2) {
  if (
    (typeof val1 === 'number' && typeof val2 === 'number') ||
    (typeof val1 === 'string' && typeof val2 === 'string')
  ) {
    return val1 + val2;
  }

  throw new Error('Must be of type String or Number');
}
```

**適當的：**
```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 別過度優化
現代瀏覽器在運行時幫你做了很多優化。大多數的情況，你所做的優化都是在浪費你的時間。這裏有些很好的[資源](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)，去了解哪些優化是無用的。

**糟糕的：**
```javascript
// 在舊的瀏覽器中並不會快取（cache）`list.length`，每次迭代（iteration）時的重新計算相當損耗效能。
// 這在新瀏覽器中已被優化，你不用手動去快取。
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**適當的：**
```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**譯者附註**

簡單來說，你不用在意程式語言層面上優化，因為這部分會因為版本更新而得到優化。但不要因此放棄所有的優化，演算法的優化才是你該注意的地方！

**[⬆ 回到目錄](#目錄table-of-contents)**

### 移除無用的程式碼（Dead Code）
沒有任何理由保留無用的程式碼，如果他們沒有被使用到，移除它！讓它們被保留在版本歷史中。

**糟糕的：**
```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**適當的：**
```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker('apples', req, 'www.inventory-awesome.io');
```

**[⬆ 回到目錄](#目錄table-of-contents)**

## 物件（Objects）與資料結構（Data Structures）
### 使用 getters 與 setters
使用 `getters` 與 `setters` 來存取物件中資料，會比單純使用屬性（property）來的好。因為：
  * 當你想要在取得物件屬性時做更多事情，你不用找出所有的程式碼修改。
  * 透過 `set` 可以建立規則進行資料校驗。
  * 封裝內部邏輯。
  * 存取時增加日誌（logging）與錯誤處理（error handling）。
  * 你可以延遲載入你的物件屬性，像是來自伺服器的資料。

**糟糕的：**
```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**適當的：**
```javascript
function makeBankAccount() {
  // 私有變數
  let balance = 0;

  // 'getter'，經由下方的返回物件對外公開
  function getBalance() {
    return balance;
  }

  // 'setter'，經由下方的返回物件對外公開
  function setBalance(amount) {
    // ... 更新前先進行驗證
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance
  };
}

const account = makeBankAccount();
account.setBalance(100);
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 讓物件擁有私有成員（members）
可以透過閉包（closures）來私有化參數（使用於 ES5 以下）。

**糟糕的：**
```javascript
const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee('John Doe');
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**適當的：**
```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    }
  };
}

const employee = makeEmployee('John Doe');
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```

**[⬆ 回到目錄](#目錄table-of-contents)**

## 類別（Classes）
### 類別語法偏好使用 ES2015/ES6 的類別更甚於 ES5 函數
ES5 的類別定義非常難以閱讀、繼承、建造與定義方法。假設你需要繼承（請注意你有可能不需要），偏好使用 ES2015/ES6 的類別。除非你需要大型且複雜的物件，不然使用小型函數會比類別更好。

**糟糕的：**
```javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error('Instantiate Animal with `new`');
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error('Instantiate Mammal with `new`');
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error('Instantiate Human with `new`');
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**適當的：**
```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() {
    // ...
  }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() {
    // ...
  }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() {
    // ...
  }
}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 使用方法鏈（method chaining）
這個模式（pattern）在 JavaScript 中非常有用，你可以在很多函數庫中看到，像是 jQuery 與 Lodash。它可以讓你的程式碼表達的更好。

基於這個原因，方法鏈可以讓你的程式碼看起來更加簡潔。在你的類別函數中，只需要回傳 `this` 在每一個函數中，你就可以鏈結所有類別中的方法。

**糟糕的：**
```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car('Ford', 'F-150', 'red');
car.setColor('pink');
car.save();
```

**適當的：**
```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // 注意：回傳 this 以鏈結
    return this;
  }

  setModel(model) {
    this.model = model;
    // 注意：回傳 this 以鏈結
    return this;
  }

  setColor(color) {
    this.color = color;
    // 注意：回傳 this 以鏈結
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // 注意：回傳 this 以鏈結
    return this;
  }
}

const car = new Car('Ford', 'F-150', 'red').setColor('pink').save();
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 偏好組合（composition）更甚於繼承（inheritance）
正如四人幫的[設計模式](https://en.wikipedia.org/wiki/Design_Patterns)，可以的話你應該優先使用組合而不是繼承。有許多好理由去使用繼承或是組合。重點是，如果你主觀認定是繼承，嘗試想一下組合能否替問題帶來更好的解法。你應該偏好使用組合更甚於繼承。

什麼時候使用繼承？這取決於你手上的問題，不過這有一些不錯的參考，說明什麼什麼時候繼承比組合更好用：

1. 你的繼承為是一種（is-a）的關係，而不是有一個（has-a）。例如「人類是一種動物 Human -> Animal」vs.「使用者有一個使用者資料 User -> UserDetails」。
2. 你能重複使用基類（base classes）的程式碼。例如，人類能像動物一樣移動。
3. 你希望能通過修改基類的程式碼來進行全域修改。例如，改變所有動物移動時的熱量消耗。

**糟糕的：**
```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// 因為僱員有稅率金資料，而不是一種僱員
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**適當的：**
```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

## 物件導向基本原則（SOLID）
### 單一功能原則 Single Responsibility Principle (SRP)
正如 Clean Code 所述：「永遠不要有超過一個理由來修改一個類型」。給一個類別塞滿許多功能，如同你在航班上只能帶一個行李箱一樣。這樣做的問題是，你的類別不會有理想的內聚性，將會有太多理由來對它進行修改。最小化需要修改一個類別的次數很重要，因為一個類別有太多的功能的話，一旦你修改一小部分，將會很難弄清楚它會對程式碼的其他模組造成什麼影響。

**糟糕的：**
```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**適當的：**
```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 開閉原則 Open/Closed Principle (OCP)
Bertrand Meyer 說過，「軟體實體（類別、模組、函數）應為開放擴展，但是關閉修改」。這原則基本上說明你應該同意使用者增加功能，而不用修改現有程式碼。

**糟糕的：**
```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'ajaxAdapter';
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'nodeAdapter';
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === 'ajaxAdapter') {
      return makeAjaxCall(url).then(response => {
        // 轉換回應並回傳
      });
    } else if (this.adapter.name === 'nodeAdapter') {
      return makeHttpCall(url).then(response => {
        // 轉換回應並回傳
      });
    }
  }
}

function makeAjaxCall(url) {
  // 發送請求並回傳 promise
}

function makeHttpCall(url) {
  // 發送請求並回傳 promise
}
```

**適當的：**
```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'ajaxAdapter';
  }

  request(url) {
    // 發送請求並回傳 promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'nodeAdapter';
  }

  request(url) {
    // 發送請求並回傳 promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then(response => {
      // transform response and return
    });
  }
}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 里氏替換原則 Liskov Substitution Principle (LSP)
這是一個驚人但簡單的概念，正式的定義為：「假如類別 S 是類別 T 的子類別，那麼類別 T 的物件（Object）可以被替換成類別 S 的物件（例如，類別 S 的物件可作為類別 T 的物件的替代品），而不需要改變任何程式的屬性（正確性、被執行的任務等）。

最好的解釋是這樣，假如你有父類別與子類別，父類別與子類別可以互換，而沒有問題發生。讓我們看看一個經典的正方形與長方形的案例。在數學上，正方形是長方形的一種，但如果你使用是一種（is-a）的關係用繼承來實現，你很快會發現問題。

**糟糕的：**
```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach(rectangle => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // 糟糕：結果為 25，應該為 20 才正確
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**適當的：**
```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach(shape => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**譯者附註**

`setWidth` 與 `setHeight` 方法無法被繼承，使用上的不一致，更造成了結果錯誤。

**[⬆ 回到目錄](#目錄table-of-contents)**

### 介面隔離原則 Interface Segregation Principle (ISP)
JavaScript 沒有介面（interfaces），所以這個原則比較不像其他語言一樣嚴格。不過它在 JavaScript  這種缺少類型的語言來說一樣重要。

ISP 原則是「客戶端不應該被強制依賴他們不需要的介面。」因為 JavaScript 是一種弱型別的語言，所以介面是一種隱式（implicit）的協議。

巨大的設定物件（objects）是這個原則的好範例，不需要客戶去設定大量的選項是有好處的，因為多數的情況下，他們不需要全部的設定。讓他們可以被選擇，可以防止出現一個過胖的介面。

**糟糕的：**
```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName('body'),
  animationModule() {} // 大多數的情況下，執行 traverse 時，我們其實不需要使用 animation。
  // ...
});
```

**適當的：**
```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName('body'),
  options: { // animationModule 可被選擇要不要使用
    animationModule() {}
  }
});
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 依賴反轉原則 Dependency Inversion Principle (DIP)
這原則說明兩個必要事情：
  1. 高層級的模組（modules）不應該依賴於低層級的模組。它們兩者必須依賴於抽象。
  2. 抽象（abstract）不應該依賴於具體實現（implement），具體實現則應依賴於抽象。

這原則一開始很難理解，但如果你使用過 AngularJS，你應該已經知道，使用依賴注入（Dependency Injection）來實現這個原則。雖然不是同一種概念，但透過依賴注入讓高層級模組遠離低層級模組的細節與設定。這樣做的巨大好處是，降低模組間的耦合。耦合是很糟的開發模式，因為會導致程式碼難以重構（refactor）。

如上所示，JavaScript 沒有介面，所以被依賴的抽象是隱式協議。也是就說，一個物件/類別的屬性直接暴露給另外一個。在以下的範例中，任何的請求模組（Request Module）的隱式協議 `InventoryTracker` 都會有一個 `requestItems` 的方法。

**糟糕的：**
```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ['HTTP'];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // 糟糕的：我們建立了一種依賴，依賴於特定（InventoryRequester）請求模組的實現。
    // 我們實際上只有 `requestItems` 方法依賴於名為 `request` 的請求方法。
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(['apples', 'bananas']);
inventoryTracker.requestItems();
```

**適當的：**
```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ['HTTP'];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ['WS'];
  }

  requestItem(item) {
    // ...
  }
}

// 通過外部創建時將依賴注入，我們可以輕鬆地用全新的 WebSockets 的請求模組替換。
const inventoryTracker = new InventoryTracker(
  ['apples', 'bananas'],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**譯者附註**

在糟糕的案例中，`InventoryTracker` 沒有提供替換請求模組的可能，`InventoryTracker` 依賴於 `InventoryRequester`。造成耦合，測試將會被難以撰寫，修改程式碼時將會牽一髮而動全身。

**[⬆ 回到目錄](#目錄table-of-contents)**

##  測試（Testing）
測試比發布更加重要。當發布時，如果你沒有測試或是測試不夠充分，你會無法確認有沒有任何功能被破壞。測試的量，由團隊決定，但是擁有 100% 的測試覆蓋率（包含狀態與分支），是你為什麼能有高度自信與內心平靜的原因。所以你需要一個偉大的測試框架，也需要一個[好的覆蓋率（coverage）工具](http://gotwarlost.github.io/istanbul/)。

沒有任何藉口不寫測試。這裡有很多[好的 JS 測試框架](http://jstherightway.org/#testing-tools)，選一個你的團隊喜歡的。選擇好之後，接下來的目標是為任何新功能或是模組撰寫測試。如果你喜好[測試驅動開發（Test Driven Development）](https://en.wikipedia.org/wiki/Test-driven_development)的方式，那就太棒了，重點是確保上線前或是重構之前，達到足夠的覆蓋率。

**譯者附註**

測試是一種保障，當你趕著修正錯誤時，測試會告訴你會不會改了 A 壞了 B。確保每次上線前的功能皆可正常運作。另外測試有分種類，詳情見連結[測試的種類](https://en.wikipedia.org/wiki/Software_testing)。

### 每個測試只測試一個概念

**糟糕的：**
```javascript
import assert from 'assert';

describe('MakeMomentJSGreatAgain', () => {
  it('handles date boundaries', () => {
    let date;

    date = new MakeMomentJSGreatAgain('1/1/2015');
    date.addDays(30);
    assert.equal('1/31/2015', date);

    date = new MakeMomentJSGreatAgain('2/1/2016');
    date.addDays(28);
    assert.equal('02/29/2016', date);

    date = new MakeMomentJSGreatAgain('2/1/2015');
    date.addDays(28);
    assert.equal('03/01/2015', date);
  });
});
```

**適當的：**
```javascript
import assert from 'assert';

describe('MakeMomentJSGreatAgain', () => {
  it('handles 30-day months', () => {
    const date = new MakeMomentJSGreatAgain('1/1/2015');
    date.addDays(30);
    assert.equal('1/31/2015', date);
  });

  it('handles leap year', () => {
    const date = new MakeMomentJSGreatAgain('2/1/2016');
    date.addDays(28);
    assert.equal('02/29/2016', date);
  });

  it('handles non-leap year', () => {
    const date = new MakeMomentJSGreatAgain('2/1/2015');
    date.addDays(28);
    assert.equal('03/01/2015', date);
  });
});
```

**譯者附註**

如果你單個測試，測試過多的功能或是概念，當這個測試出錯的時候，你將會難以找到出錯的程式碼。

**[⬆ 回到目錄](#目錄table-of-contents)**

## 並發（Concurrency）
### 使用 Promises，不要使用回呼函式（callback）
回呼函式不怎麼簡潔，他們會導致過多的巢狀。在 ES2016/ES6，Promises 已經是內建的全局類型（global type）。使用它們吧！

**糟糕的：**
```javascript
import { get } from 'request';
import { writeFile } from 'fs';

get(
  'https://en.wikipedia.org/wiki/Robert_Cecil_Martin',
  (requestErr, response) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile('article.html', response.body, writeErr => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log('File written');
        }
      });
    }
  }
);
```

**適當的：**

```javascript
import { get } from 'request';
import { writeFile } from 'fs';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then(response => {
    return writeFile('article.html', response);
  })
  .then(() => {
    console.log('File written');
  })
  .catch(err => {
    console.error(err);
  });
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### Async/Await 比 Promises 更加簡潔
Promises 是回呼函式的一種非常簡潔的替代品，但是 ES2017/ES8 帶來了 async 與 await，提供了一個更簡潔的方案。你需要的只是一個前綴為 `async` 關鍵字的函數，接下來你編寫邏輯時就不需要使用 `then` 函數鍊。如果你能使用 ES2017/ES8 的進階功能的話，今天就使用它吧！

**糟糕的：**
```javascript
import { get } from 'request-promise';
import { writeFile } from 'fs-promise';

get('https://en.wikipedia.org/wiki/Robert_Cecil_Martin')
  .then(response => {
    return writeFile('article.html', response);
  })
  .then(() => {
    console.log('File written');
  })
  .catch(err => {
    console.error(err);
  });
```

**適當的：**
```javascript
import { get } from 'request-promise';
import { writeFile } from 'fs-promise';

async function getCleanCodeArticle() {
  try {
    const response = await get(
      'https://en.wikipedia.org/wiki/Robert_Cecil_Martin'
    );
    await writeFile('article.html', response);
    console.log('File written');
  } catch (err) {
    console.error(err);
  }
}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

## 錯誤處理（Error Handling）
拋出錯誤是一件好事情！代表運行時可以成功辨識程式中的錯誤，通過停止執行目前堆疊（stack）上的執行函數，結束（Node.js 中的）目前程序（process），並在控制台中用一個堆疊追蹤（stack trace）提醒你。

### 不要忽略捕捉到的錯誤
捕捉到一個錯誤時而不做任何處理，會讓你失去修復或是反應錯誤的能力。將錯誤紀錄於控制台（`console.log`）也不怎麼好，因為你往往會迷失在控制台大量的記錄之中。如果你使用 `try/catch` 包住程式碼，代表你預期這裡可能會出錯，因此當錯誤發生時你必須要有個處理方法。

**糟糕的：**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**適當的：**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // 可以這樣（會比 console.log 更吵）
  console.error(error);
  // 或這種方法
  notifyUserOfError(error);
  // 另外一種方法
  reportErrorToService(error);
  // 或是全部都做！
}
```

### 不要忽略被拒絕的 Promises
原因如上節所述，不要忽略任何捕捉到的錯誤。

**糟糕的：**
```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**適當的：**
```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    // 可以這樣（會比 console.log 更吵）
    console.error(error);
    // 或這種方法
    notifyUserOfError(error);
    // 另外一種方法
    reportErrorToService(error);
    // 或是全部都做！
  });
```

**[⬆ 回到目錄](#目錄table-of-contents)**

## 格式化（Formatting）
格式化是很主觀的，就像其他規則一樣沒有硬性規定，沒有必要為了格式而爭論，這裡有[大量的自動化格式工具](https://standardjs.com/rules.html)，選一個就是了！對工程師來說，爭論格式就是在浪費時間與金錢。

針對自動格式化工具不能涵蓋的問題，這裡有一些指南。

### 使用一致的大小寫
JavaScript 是動態型別的語言，所以從大小寫可以看出關於變數、函數等很多的事情。這些規則是很主觀的，所以你的團隊可以自由選擇。重點是，不管選了去什麼，就保持一致。

**糟糕的：**
```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const Artists = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**適當的：**
```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ['Back In Black', 'Stairway to Heaven', 'Hey Jude'];
const ARTISTS = ['ACDC', 'Led Zeppelin', 'The Beatles'];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 函數的呼叫者應該與被呼叫者靠近
如果一個函數呼叫另外一個，在程式碼中兩個函數的垂直位置應該靠近。理想情況下，呼叫函數應於被呼叫函數的正上方。我們傾向於從上到下的閱讀方式，就像看報紙一樣。基於這個原因，讓你的程式碼可以依照這種方式閱讀。

**糟糕的：**
```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, 'peers');
  }

  lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**適當的：**
```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, 'peers');
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, 'manager');
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**[⬆ 回到目錄](#目錄table-of-contents)**

## 註解（Comments）
### 只對商業邏輯複雜的部分撰寫註解
註解是代表的辯解，而不是要求。多數情況下，好的程式碼本身就是文件。

**糟糕的：**
```javascript
function hashIt(data) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = (hash << 5) - hash + char;
    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**適當的：**

```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 不要在程式碼中保留被註解掉的程式碼
有了版本控制，舊的程式碼留在歷史紀錄中就好。

**糟糕的：**
```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**適當的：**
```javascript
doStuff();
```

**[⬆ 回到目錄](#目錄table-of-contents)**

### 不要留有日誌式的註解
記住，使用版本控制！不需要無用的、註解掉的程式碼，尤其是日誌式的註解。使用 `git log` 來保存歷史紀錄。

**糟糕的：**
```javascript
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**適當的：**
```javascript
function combine(a, b) {
  return a + b;
}
```

**譯者附註**

在註解中寫歷史紀錄並沒有在版本控制中來得有效，另外補充有關歷史紀錄如何撰寫的規範（[如何撰寫 Git Commit Message](https://chris.beams.io/posts/git-commit/)）。

**[⬆ 回到目錄](#目錄table-of-contents)**

### 避免位置標示
它們只會增加干擾。讓函數與變數的名稱沿著合適的縮排與格式化，為你的程式碼帶來良好的視覺結構。

**糟糕的：**
```javascript
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
  // ...
};
```

**適當的：**
```javascript
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

const actions = function() {
  // ...
};
```

**[⬆ 回到目錄](#目錄table-of-contents)**

## 翻譯（Translation）
以下為所有的翻譯版本。

- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**:
  [GavBaros/clean-code-javascript-fr](https://github.com/GavBaros/clean-code-javascript-fr)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Spanish**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Simplified Chinese**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Traditional Chinese**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Indonesia**:
  [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**:
  [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)

**[⬆ 回到目錄](#目錄table-of-contents)**

