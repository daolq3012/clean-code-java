
Original Repository: [ryanmcdermott/clean-code-java](https://github.com/leonardolemie/clean-code-java)

# clean-code-java

## Mục lục
  1. [Giới thiệu](#giới-thiệu)
  2. [Biến](#biến)
  3. [Hàm](#hàm)
  4. [Đối tượng và Cấu trúc dữ liệu](#Đối-tượng-và-cấu-trúc-dữ-liệu)
  5. [Lớp](#lớp)
  6. [SOLID](#solid)
  7. [Testing](#testing)
  8. [Xử lí đồng thời](#xử-lí-đồng-thời)
  9. [Xử lí lỗi](#xử-lí-lỗi)
  10. [Định dạng](#Định-dạng)
  11. [Viết chú thích](#viết-chú-thích)
  12. [Các ngôn ngữ khác](#các-ngôn-ngữ-khác)

## Giới thiệu
![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](http://www.osnews.com/images/comics/wtfm.jpg)

Những nguyên tắc kỹ thuật phần mềm, từ cuốn sách
[*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
áp dụng cho ngôn ngữ Java. Đây không phải là một hướng dẫn về cách viết code Java mà là hướng dẫn về cách viết các đoạn code dễ đọc hiểu, tái sử dụng và tái cấu trúc được trong Java.

Không phải mọi nguyên tắc ở đây phải được tuân thủ một cách nghiêm ngặt,
và thậm chí chỉ có một ít trong số đó được sử dụng phổ biến. Ở đây, nó chỉ là một
hướng dẫn - không hơn không kém, nhưng chúng được hệ thống hóa thông qua kinh
nghiệm thu thập được qua nhiều năm của các tác giả của cuốn sách *Clean Code*

Ngành kỹ thuật phần mềm chỉ phát triển được hơn 50 năm, và chúng ta vẫn
đang học rất nhiều. Một khi kiến trúc phần mềm trở thành phổ biến, có lẽ sau đó
chúng ta sẽ có thêm nhiều luật lệ khó hơn phải tuân theo. Còn giờ đây,
hãy để những hướng dẫn này như là một tiêu chuẩn để đánh giá chất lượng các đoạn
code Java mà bạn và team của bạn tạo ra.

Biết những hướng dẫn này thôi sẽ không thể ngay lập tức làm bạn trở thành một
lập trình viên phần mềm tốt hơn được, và làm việc với chúng trong nhiều năm
cũng không có nghĩa bạn sẽ không gặp bất cứ sai lầm nào. Mỗi đoạn code bắt đầu
như một bản thảo đầu tiên, giống như đất sét được nặn nhào và cho tới cuối cùng
thì nó sẽ lộ diện hình hài. Cuối cùng, chúng ta gọt tỉa những khuyết điểm khi
chúng ta xem xét lại nó cùng với các đồng nghiệp.
Đừng để bản thân bạn bị đánh bại bởi những bản thảo đầu tiên,
thứ mà vẫn cần phải được chỉnh sửa. Thay vào đó hãy đánh bại những dòng code.

## **Biến**
### Sử dụng tên biến có nghĩa và dễ phát âm

**Không tốt:**
```java
String yyyymmdstr = new SimpleDateFormat("YYYY/MM/DD").format(new Date());
```

**Tốt:**
```java
String currentDate = new SimpleDateFormat("YYYY/MM/DD").format(new Date());
```
**[⬆ về đầu trang](#mục-lục)**

### Sử dụng cùng từ vựng cho cùng loại biến

**Không tốt:**
```java
getUserInfo();
getClientData();
getCustomerRecord();
```

**Tốt:**
```java
getUser();
```


**[⬆ về đầu trang](#mục-lục)**

### Sử dụng các tên có thể tìm kiếm được
Chúng ta sẽ đọc code nhiều hơn là viết chúng. Điều quan trọng là code chúng ta
viết có thể đọc được và tìm kiếm được. Việc đặt tên các biến *không* có ngữ
nghĩa so với chương trình, chúng ta có thể sẽ làm người đọc code bị tổn thương
tinh thần.
Hãy làm cho các tên biến của bạn có thể tìm kiếm được.

**Không tốt:**
```java
// 86400000 là cái quái gì thế?
setTimeout(blastOff, 86400000);

```

**Tốt:**
```java
// Khai báo chúng như một biến hằng global.
public static final int MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);

```
**[⬆ về đầu trang](#mục-lục)**

### Sử dụng những biến có thể giải thích được
**Không tốt:**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(address.match(cityZipCodeRegex)[1], address.match(cityZipCodeRegex)[2]);
```

**Tốt:**
```javascript
const address = 'One Infinite Loop, Cupertino 95014';
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ về đầu trang](#mục-lục)**

### Tránh hại não người khác
Tường minh thì tốt hơn là ẩn.

**Không tốt:**
```java
String [] l = {"Austin", "New York", "San Francisco"};

for (int i = 0; i < l.length; i++) {
    String li = l[i];
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    // Khoan, `l` là cái gì vậy?
    dispatch(li);
 }
```

**Tốt:**

```java
String[] locations = {"Austin", "New York", "San Francisco"};

for (String location : locations) {
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    dispatch(location);
 }
```
**[⬆ về đầu trang](#mục-lục)**

### Đừng thêm những ngữ cảnh không cần thiết
Nếu tên của lớp hay đối tượng của bạn đã nói lên điều gì đó rồi, đừng lặp lại điều đó trong tên biến nữa.

**Không tốt:**
```java
class Car {
  public String carMake = "Honda";
  public String carModel = "Accord";
  public String carColor = "Blue";
}

void paintCar(Car car) {
  car.carColor = "Red";
}
```

**Tốt:**
```java
class Car {
  public String make = "Honda";
  public String model = "Accord";
  public String color = "Blue";
}

void paintCar(Car car) {
  car.color = "Red";
}
```
**[⬆ về đầu trang](#mục-lục)**

## **Hàm**
### Đối số của hàm (lý tưởng là ít hơn hoặc bằng 2)
Giới hạn số lượng param của hàm là một điều cực kì quan trọng bởi vì nó làm cho
hàm của bạn trở nên dễ test hơn. Trường hợp có nhiều hơn 3 params có thể dẫn
đến việc bạn phải test hàng tấn test case khác nhau với những đối số riêng biệt.

1 hoặc 2 đối số là trường hợp lý tưởng, còn trường hợp 3 đối số thì nên tránh
nếu có thể. Những trường hợp khác (từ 3 params trở lên) thì nên được gộp lại.
Thông thường nếu có nhiều hơn 2 đối số thì hàm của bạn đang cố thực hiện quá
nhiều việc rồi đấy. Trong trường hợp ngược lại, phần lớn thời gian một đối
tượng cấp cao sẽ là đủ để làm đối số.


**[⬆ về đầu trang](#mục-lục)**


### Hàm chỉ nên giải quyết một vấn đề
Đây là quy định quan trọng nhất của kỹ thuật phần mềm. Khi một hàm thực hiện
nhiều hơn 1 việc, chúng sẽ trở nên khó khăn hơn để viết code, test, và suy luận.
Khi bạn có thể tách biệt một hàm để chỉ thực hiện một hành động, thì sẽ dễ dàng
hơn để tái cấu trúc và code của bạn sẽ dễ đọc hơn nhiều. Nếu bạn chỉ cần làm theo
hướng dẫn này thôi mà không cần làm gì khác thì bạn cũng đã giỏi hơn nhiều
developer khác rồi.

**Không tốt:**
```java
public void emailClients(List<Client> clients) {
    for (Client client : clients) {
        Client clientRecord = repository.findOne(client.getId());
        if (clientRecord.isActive()){
            email(client);
        }
    }
}
```

**Tốt:**
```java
public void emailClients(List<Client> clients) {
    for (Client client : clients) {
        if (isActiveClient(client)) {
            email(client);
        }
    }
}

private boolean isActiveClient(Client client) {
    Client clientRecord = repository.findOne(client.getId());
    return clientRecord.isActive();
}
```

**[⬆ Về đầu trang](#mục-lục)**

### Tên hàm phải nói ra được những gì chúng làm

**Không tốt:**
```java
private void addToDate(Date date, int month){
    //..
}

Date date = new Date();

// Khó để biết được hàm này thêm gì thông qua tên hàm.
addToDate(date, 1);
```
**Tốt:**
```java
private void addMonthToDate(Date date, int month){
    //..
}

Date date = new Date();
addMonthToDate(1, date);
```
**[⬆ Về đầu trang](#mục-lục)**

### Xóa code trùng lặp
Tuyệt đối tránh những dòng code trùng lặp. Code trùng lặp thì không tốt bởi vì
nếu bạn cần thay đổi cùng một logic, bạn phải sửa ở nhiều hơn một nơi.

Hãy tưởng tượng nếu bạn điều hành một nhà hàng và bạn theo dõi hàng tồn kho:
bao gồm cà chua, hành tây, tỏi, gia vị, vv.... Nếu bạn có nhiều danh sách
quản lý, thì tất cả chúng phải được thay đổi khi bạn phục vụ một món ăn có
chứa cà chua. Nếu bạn chỉ có 1 danh sách, thì việc cập nhật ở một nơi thôi.

Thông thường, bạn có những dòng code lặp lại bởi vì bạn có 2 hay nhiều hơn
những thứ chỉ khác nhau chút ít, mà chia sẻ nhiều thứ chung, nhưng sự khác
nhau của chúng buộc bạn phải có 2 hay nhiều hàm riêng biệt để làm nhiều điều
tương tự nhau. Xóa đi những dòng code trùng có nghĩa là tạo ra một abstraction
có thể xử lý tập những điểm khác biệt này chỉ với một hàm/module hay class.

Có được một abstraction đúng thì rất quan trọng, đó là lý do tại sao bạn nên
tuân thủ các nguyên tắc SOLID được đặt ra trong phần *Lớp*. Những abstraction
không tốt có thể còn tệ hơn cả những dòng code bị trùng lặp, vì thế hãy cẩn
thận! Nếu bạn có thể tạo ra một abstraction tốt, hãy làm nó! Đừng lặp lại chính
mình, nếu bạn không muốn đi cập nhật nhiều nơi bất cứ khi nào bạn muốn thay đổi
một thứ gì đó.

**[⬆ Về đầu trang](#mục-lục)**

### Tránh những ảnh hưởng phụ (Side Effect)
Một hàm tạo ra ảnh hưởng phụ nếu nó làm bất kì điều gì khác hơn là nhận một giá
trị đầu vào và trả về một hoặc nhiều giá trị. Ảnh hưởng phụ có thể là ghi một
file, thay đổi vài biến toàn cục, hoặc vô tình đưa tất cả tiền của bạn cho một
người lạ.

Bây giờ, cũng có khi bạn cần ảnh hưởng phụ trong một chương trình. Giống như ví dụ
trước, bạn cần ghi một file. Những gì bạn cần làm là tập trung vào nơi bạn sẽ làm
nó. Đừng viết hàm và lớp riêng biệt để tạo ra một file cụ thể. Hãy có một service
để viết nó. Một và chỉ một.

Điểm chính là để tránh những lỗi chung như chia sẻ trạng thái giữa những đối tượng
mà không có bất kì cấu trúc nào, sử dụng các kiểu dữ liệu có thể thay đổi được mà
có thể được ghi bởi bất cứ thứ gì, và không tập trung nơi có thể xảy ra các ảnh hưởng
phụ. Nếu bạn có thể làm điều đó, bạn sẽ hạnh phúc hơn so với phần lớn các lập trình
viên khác đấy.


**Không tốt:**
```javascript
// Biến toàn cục được tham chiếu bởi hàm dưới đây.
// Nếu chúng ta có một hàm khác sử dụng name, nó sẽ trở thành một array
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Tốt:**
```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(' ');
}

const name = 'Ryan McDermott';
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```
**[⬆ back to top](#mục-lục)**

### Đóng gói các điều kiện

**Không tốt:**
```java
if ('fetching'.equal(fsm.getState()) && listNode.isEmpty()) {
  // ...
}
```

**Tốt:**
```java
boolean shouldShowProgressBar(fsm, listNode) {
  return 'fetching'.equal(fsm.getState()) && listNode.isEmpty();
}

if (shouldShowProgressBar(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ back to top](#mục-lục)**

### Trách những điều kiện phủ định

**Không tốt:**
```java
boolean isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Tốt:**
```java
boolean isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ back to top](#mục-lục)**

### Tránh điều kiện
Đây dường như là một việc bất khả thi. Khi nghe điều này đầu tiên, hầu hết mọi
người đều nói, "Làm sao tôi cần phải làm gì mà không có mệnh đề `if`?"
Câu trả lời là bạn có thể sử dụng tính đa hình để đạt được công việc tương tự
trong rất nhiều trường hợp. Câu hỏi thứ hai thường là "Đó là điều tốt nhưng tại
sao tôi lại muốn làm điều đó?" Câu trả lời là khái niệm mà ta đã học ở phần
trước: một hàm chỉ nên thực hiện một việc. Khi bạn có nhiều lớp và hàm mà có
nhiều mệnh đề `if`, bạn đang cho người dùng của bạn biết rằng hàm của bạn đang
làm nhiều hơn một việc. Hãy nhớ, chỉ làm một công việc thôi.

**Không tốt:**
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

**Tốt:**
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
**[⬆ back to top](#mục-lục)**

### Xóa code chết (dead code)
Dead code cũng tệ như code trùng lặp. Không có lý do gì để giữ chúng lại trong
codebase của bạn. Nếu nó không được gọi nữa, hãy bỏ nó đi! Nó vẫn sẽ nằm trong
lịch sử phiên bản của bạn nếu bạn vẫn cần nó.

**[⬆ back to top](#mục-lục)**

## **Đối tượng và Cấu trúc dữ liệu**
### Sử dụng getter và setter
Đây là một danh sách các lí do tại sao:

* Khi bạn muốn thực hiện nhiều hơn việc lấy một thuộc tính của đối tượng, bạn không
cần phải tìm kiếm và thay đổi mỗi accessor trong codebase của bạn.
* Làm cho việc thêm các validation đơn giản khi thực hiện trên một `tập hợp`.
* Đóng gói các biểu diễn nội bộ.
* Dễ dàng thêm log và xử lí lỗi khi getting và setting.
* Kế thừa lớp này, bạn có thể override những hàm mặc định.
* Bạn có thể lazy load các thuộc tính của một đối tượng, lấy nó từ server.

**[⬆ back to top](#mục-lục)**

## **Lớp**
### Sử dụng hàm khởi tạo (constructor) quá nhiều biến hoặc gọi nhiều hàm set liên tiếp nhau
- Tránh sử dụng hàm constructor với nhiều hơn 3 đối số truyền vào thay vì vậy hãy áp dụng Builder Pattern

### Ưu tiên thành phần hơn là kế thừa
- Như đã được nhấn mạnh nhiều trong [*Design Patterns*](https://en.wikipedia.org/wiki/Design_Patterns)
của Gang of Four, bạn nên sử dụng cấu trúc thành phần hơn là thừa kế nếu có thể.
Có rất nhiều lý do tốt để sử dụng kế thừa cũng như sử dụng thành phần.
Điểm nhấn cho phương châm này đó là nếu tâm trí của bạn đi theo bản năng thừa kế,
thử nghĩ nếu thành phần có thể mô hình vấn đề của bạn tốt hơn. Trong một số trường
hợp nó có thể.

Bạn có thể tự hỏi, "khi nào tôi nên sử dụng thừa kế?" Nó phụ thuộc vào
vấn đề trong tầm tay của bạn, nhưng đây là một danh sách manh nha khi kế thừa
có ý nghĩa hơn thành phần:

1. Kế thừa của bạn đại diện cho mỗi quan hệ "is-a" và không có mỗi quan hệ "has-a"
(Human->Animal vs. User->UserDetails).
2. Bạn có thể sử dụng lại code từ lớp cơ bản (Humans có thể di chuyển giống tất cả Animals).
3. Bạn muốn làm thay đổi toàn cục đến các lớp dẫn xuất bằng cách thay đổi lớp cơ bản.

**[⬆ back to top](#mục-lục)**

## **SOLID**
### Nguyên lí đơn trách nhiệm (Single Responsibility Principle)
Như đã được nói đến trong cuốn Clean Code, "Chỉ có thể thay đổi một lớp vì một lí
do duy nhất". Thật là hấp dẫn để nhồi nhét nhiều chức năng vào cho một lớp, giống
như là khi bạn chỉ có thể lấy một chiếc vali cho chuyến bay vậy. Vấn đề là lớp của
bạn sẽ không được hiểu gắn kết về mặt khái niệm của nó và sẽ có rất nhiều lí do
để thay đổi. Việc làm giảm thiểu số lần bạn cần phải thay đổi một lớp là một việc
quan trọng. Nó quan trọng bởi vì nếu có quá nhiều chức năng trong một lớp và bạn
chỉ muốn thay đổi một chút xíu của lớp đó, thì có thể sẽ rất khó để hiểu được việc
thay đổi đó sẽ ảnh hưởng đến những module khác trong codebase như thế nào.

**Không tốt:**
```java
class UserSettings {
  private User user;
  UserSettings(User user) {
    this.user = user;
  }

  void changeSettings(UserSettings settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  boolean verifyCredentials() {
    // ...
  }
}
```

**Tốt:**
```java
class UserAuth {
  private User user;
  UserAuth(User user) {
    this.user = user;
  }

  boolean verifyCredentials() {
    // ...
  }
}


class UserSettings {
  private User user;
  UserSettings(User user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  void changeSettings(UserSettings settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```
**[⬆ back to top](#mục-lục)**

### Nguyên lí đóng mở (Open/Closed Principle)
Betrand Meyer đã nói "có thể thoải mái mở rộng một module, nhưng hạn chế sửa
đổi bên trong module đó". Điều đó nghĩa là gì? Nguyên tắc này cơ bản nhấn mạnh
rằng bạn phải cho phép người dùng thêm các chức năng mới mà không làm thay
đổi các code đang có.

**Không tốt:**
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
      return makeAjaxCall(url).then((response) => {
        // transform response and return
      });
    } else if (this.adapter.name === 'httpNodeAdapter') {
      return makeHttpCall(url).then((response) => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url) {
  // request and return promise
}

function makeHttpCall(url) {
  // request and return promise
}
```

**Tốt:**
```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'ajaxAdapter';
  }

  request(url) {
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = 'nodeAdapter';
  }

  request(url) {
    // request and return promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then((response) => {
      // transform response and return
    });
  }
}
```

**[⬆ back to top](#mục-lục)**

### Nguyên lí thay thế Liskov (Liskov Substitution Principle)
Đây là một thuật ngữ đáng sợ cho một khái niệm rất đơn giản. Nó được định nghĩa
một cách chính thức là: "Nếu S là một kiểu con của T, thì các đối tượng của kiểu
T có thể được thay thế bằng các đối tượng của kiểu S (ví dụ các đối tượng của
kiểu S có thể thay thế các đối tượng của kiểu T) mà không làm thay đổi bất kì thuộc
tính mong muốn nào của chương trình đó (tính chính xác, thực hiện tác vụ, ..).
Đó thậm chí còn là một định nghĩa đáng sợ hơn.

Sự giải thích tốt nhất cho nguyên lí này là, nếu bạn có một lớp cha và một lớp con,
thì lớp cơ sở và lớp con có thể được sử dụng thay thế cho nhau mà không làm thay
đổi tính đúng đắn của chương trình. Có thể vẫn còn hơi rối ở đây, vậy hãy xem
cái ví dụ cổ điển hình vuông-hình chữ nhật (Square-Rectangle) dưới đây. Về mặt
toán học, một hình vuông là một hình chữ nhật, tuy nhiên nếu bạn mô hình hoá điều
này sử dụng quan hệ "is a" thông qua việc kế thừa, bạn sẽ nhanh chóng gặp phải
rắc rối đấy.

**Không tốt:**
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
  rectangles.forEach((rectangle) => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // BAD: Will return 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Tốt:**
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
  shapes.forEach((shape) => {
      const area = shape.getArea();
      shape.render(area);
    });
  }

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ back to top](#mục-lục)**

### Nguyên lí phân tách interface (Interface Segregation Principle)
Nguyên lí phân tách interface nhấn mạnh rằng "Người dùng không nên bị bắt
buộc phải phụ thuộc vào các interfaces mà họ không sử dụng."

Một ví dụ tốt để minh hoạ cho nguyên lí này trong Java là các lớp mà yêu
cầu cài đặt các đối tượng lớn. Việc không yêu cầu người dùng thiết lập một số
lượng lớn các phương thức là một điều tốt, bởi vì đa số họ không cần tất
cả các cài đặt. Làm cho chúng trở thành tuỳ chọn giúp tránh được việc có một
"fat interface".


**[⬆ back to top](#mục-lục)**

### Nguyên lí đảo ngược dependency (Dependency Inversion Principle)
Với cách code thông thường, các module cấp cao sẽ gọi các module cấp thấp. 
Module cấp cao sẽ phụ thuộc và module cấp thấp, điều đó tạo ra các dependency. 
Khi module cấp thấp thay đổi, module cấp cao phải thay đổi theo. 
Một thay đổi sẽ kéo theo hàng loạt thay đổi, giảm khả năng bảo trì của code.

Nguyên lí này khẳng định hai điều cần thiết sau:
1. Nhưng module cấp cao không nên phụ thuộc vào những module cấp thấp. Cả
hai nên phụ thuộc vào abstraction.
2. Abstraction (interface) không nên phụ thuộc vào chi tiết, mà ngược lại.

Điều này có thể khó hiểu lúc ban đầu, nhưng nếu bạn đã từng làm việc với Angular.js,
bạn đã thấy một sự hiện thực của nguyên lí này trong dạng của Dependency Injection
(DI). Khi chúng không phải là các khái niệm giống nhau, DIP giữ cho module cấp
cao không biết chi tiết các module cấp thấp của nó và thiết lập chúng. Có thể đạt
được điều này thông qua DI. Một lợi ích to lớn của DIP là nó làm giảm sự phụ thuộc
lẫn nhau giữa các module. Sự phụ thuộc lẫn nhau là một kiểu mẫu không tốt, vì nó
làm cho việc tái cấu trúc code trở nên khó khăn.

**Không tốt:**
```java
// Cart là module cấp cao
public class Cart
{
    public void Checkout(int orderId, int userId)
    {
        // Database, Logger, EmailSender là module cấp thấp
        Database db = new Database();
        db.Save(orderId);
 
        Logger log = new Logger();
        log.LogInfo("Order has been checkout");
 
        EmailSender es = new EmailSender();
        es.SendEmail(userId);
    }
}
```

**Tốt:**
```java
// Interface
public interface IDatabase
{
    void Save(int orderId);
}
public interface ILogger
{
    void LogInfo(string info);
}
public interface IEmailSender
{
    void SendEmail(int userId);
}
 
// Các Module implement các Interface
public class Logger : ILogger
{
    public void LogInfo(string info) {}
}
public class Database : IDatabase
{
    public void Save(int orderId) {}
}
public class EmailSender : IEmailSender
{
    public void SendEmail(int userId) {}
}

// Hàm checkout mới sẽ như sau
public void Checkout(int orderId, int userId)
{
    // Nếu muốn thay đổi database, logger ta chỉ cần thay dòng code dưới
    // Các Module XMLDatabase, SQLDatabase phải implement IDatabase
    //IDatabase db = new XMLDatabase(); 
    //IDatebase db = new SQLDatabase();
    IDatabase db = new Database();
    db.Save(orderId);
 
    ILogger log = new Logger();
    log.LogInfo("Order has been checkout");
 
    IEmailSender es = new EmailSender();
    es.SendEmail(userId);
}
```
Trong thực tế, người ta thường áp dụng pattern DI (Dependency Injection) để đảm bảo nguyên lý DIP (Dependency Inversion Principle) trong code.
**[⬆ back to top](#mục-lục)**

## **Testing**
Testing thì quan trọng hơn shipping. Nếu bạn không có test hoặc không đủ,
thì mỗi lần ship code bạn sẽ không chắc là mình có làm hư hại thứ gì không.
Việc quyết định những gì để tạo thành số lượng test đủ là do team của bạn,
nhưng việc có 100% độ bao phủ (tất cả các câu lệnh và rẽ nhánh) là cách để
bạn đạt được sự tự tin cao. Điều này có nghĩa ngoài việc có được một framework
để test tốt, bạn cũng cần sử dụng một công cụ tính độ bao phủ tốt

Không có lí do gì để không viết test. Có rất nhiều framework test Java tốt,
vì thế hãy tìm một framework mà team bạn thích. Khi đã tìm được một cái thích
hợp với team của mình, hãy đặt mục tiêu để luôn luôn viết test cho mỗi tính
năng hoặc module mới của bạn. Nếu phương pháp test ưa thích của bạn là Test
Driven Development (TDD), điều đó thật tuyệt, nhưng điểm quan trọng là phải
chắc chắn bạn đạt được mục tiêu về độ bao phủ trước khi launch một tính năng
hoặc refactor một tính năng cũ nào đó.

**[⬆ back to top](#mục-lục)**

## **Xử lí đồng thời**
### Hãy dùng higher-order function, đừng dùng callback tránh callback hell
Tham khảo thư viện [ReactiveX/RxJava](https://github.com/ReactiveX/RxJava)

**[⬆ back to top](#mục-lục)**

## **Xử lí lỗi**
Thông báo lỗi là một điều tốt! Nghĩa là chương trình của bạn nhận dạng
được khi có một cái gì đó chạy không đúng và nó sẽ cho bạn biết bằng việc
dừng chức năng mà nó đang thực thi và thông
báo cho bạn trong console với một log để theo dấu.

### Đừng bỏ qua những lỗi đã bắt được
Nếu không làm gì với lỗi đã bắt được, bạn sẽ không thể sửa hoặc phản ứng
lại được với lỗi đó. Ghi lỗi ra console (`console.log`) cũng không tốt hơn
bao nhiêu vì đa số nó có thể bị trôi mất trong một đống những thứ được hiển
thị ra ở console. Nếu bạn đặt bất cứ đoạn code nào trong một block `try/catch`,
tức là bạn nghĩ một lỗi có thể xảy ra ở đây, do đó bạn nên có một giải pháp
hoặc tạo một luồng code để xử lí lỗi khi nó xảy ra.

**Không tốt:**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Tốt:**
```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
}
```

**[⬆ back to top](#mục-lục)**

## **Định dạng**
Việc định dạng code mang tính chủ quan. Giống như nhiều quy tắc được trình
bày trong tài liệu này, không có quy tắc nào cứng nhắc và nhanh chóng mà bạn
bắt buộc phải tuân theo. Điểm chính của phần này là ĐỪNG BAO GIỜ TRANH CÃI
về việc định dạng code như thế nào. Thật tốn thời gian và
tiền bạc chỉ để tranh cãi về vấn đề định dạng code.

Đối với những thứ không thuộc phạm vi của việc tự động định dạng code (thụt đầu
dòng, tab và space, nháy đơn và nháy kép,..) hãy xem một số hướng dẫn ở [đây](https://github.com/LobeSoftware/coding-standards/blob/master/Android.md).

### Các hàm gọi và hàm được gọi nên nằm gần nhau
Nếu một hàm gọi một hàm  khác, hãy giữ những hàm này nằm gần theo chiều dọc trong
file. Lí tưởng là, hãy giữ cho hàm gọi ở trên hàm được gọi. Chúng ta có xu hướng
đọc code từ trên xuống, giống như đọc báo vậy. Do đó, hãy làm cho code của chúng
ta cũng được đọc theo cách đó.

**[⬆ back to top](#mục-lục)**

## **Viết chú thích**
### Chỉ nên viết chú thích cho những thứ có logic phức tạp.
Các chú thích thường là lời xin lỗi, chứ không phải là yêu cầu.
Những đoạn code tốt thì *đa số* tự nó đã là tài liệu rồi.

**Không tốt:**
```javascript
function hashIt(data) {
  // Khai báo hash
  let hash = 0;

  // Lấy chiều dài của chuỗi
  const length = data.length;

  // Lặp qua mỗi kí tự
  for (let i = 0; i < length; i++) {
    // Lấy mã của kí tự
    const char = data.charCodeAt(i);
    // Gán giá trị cho hash
    hash = ((hash << 5) - hash) + char;
    // Chuyển thành định dạng số nguyên 32 bit
    hash &= hash;
  }
}
```

**Tốt:**
```javascript

function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = ((hash << 5) - hash) + char;

    // Chuyển thành định dạng số nguyên 32 bit
    hash &= hash;
  }
}

```
**[⬆ back to top](#mục-lục)**

### Đừng giữ lại những đoạn code bị chú thích
Những công cụ quản lí phiên bản sinh ra để làm nhiệm vụ của chúng.
Hãy để code cũ của bạn nằm lại trong dĩ vãng đi.

**Không tốt:**
```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Tốt:**
```javascript
doStuff();
```
**[⬆ back to top](#mục-lục)**

### Đừng viết các chú thích nhật ký.
Hãy nhớ, sử dụng công cụ quản lí phiên bản như Git! Chúng ta không cần những đoạn code
vô dụng, bị chú thích và đặc biệt là những chú thích dạng nhật ký...
Sử dụng `git log` để xem lịch sử được mà!

**Không tốt:**
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

**Tốt:**
```javascript
function combine(a, b) {
  return a + b;
}
```
**[⬆ back to top](#mục-lục)**

### Tránh những đánh dấu vị trí
Chúng thường xuyên làm nhiễu code. Hãy để những tên hàm, biến cùng với các
định dạng thích hợp tự tạo thành cấu trúc trực quan cho code của bạn.

**Không tốt:**
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

**Tốt:**
```javascript
$scope.model = {
  menu: 'foo',
  nav: 'bar'
};

const actions = function() {
  // ...
};
```
**[⬆ back to top](#mục-lục)**
