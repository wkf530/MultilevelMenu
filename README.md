# MultilevelMenu
A custom multi-level menu/ address picker.
***
![gif1](https://github.com/ChokShen/MultilevelMenu/raw/master/Screenshots/MultilevelStyle1Menu.gif)  
***MultilevelStyle1Menu*** 
 
![gif2](https://github.com/ChokShen/MultilevelMenu/raw/master/Screenshots/MultilevelStyle2Menu.gif)  
***MultilevelStyle2Menu***

## Requirements 
* iOS 8+
* Xcode 11+
* Swift 5.0+

## Installation 
### 1.CocoaPods
```swift
pod 'MultilevelMenu'
```
### 2.Manually
Download the project, then drag the files of MultilevelMenu folder to your project.

## Basic usage
### 1.Data structure
```swift
open class MenuDataModel {
    ///Id of a piece of data in menu，required
    open var id: String?
    ///Data name，required
    open var name: String?
    ///Data value，optional
    open var value: String?
    ///Id of last-level menu，required
    open var pid: String?
    ///Menu current level，optional
    open var level: Int?

    public init() {}

    public init(dict: [AnyHashable: Any]) {
        self.id = dict["id"] as? String
        self.name = dict["name"] as? String
        self.value = dict["value"] as? String
        self.pid = dict["pid"] as? String
        self.level = dict["level"] as? Int
    }
}
```
### 2.Init

Data handle
```swift
let array: [Dictionary<String, String>] = [
    ["id":"01","name":"实物产品行业","value":""],
    ["id":"02","name":"服务行业","value":""],
    ["id":"0101","name":"综合超市/卖场","pid":"01","value":""],
    ["id":"0102","name":"穿戴用品","pid":"01","value":""],
    ["id":"0103","name":"饮食产品","pid":"01","value":""],
    ["id":"0104","name":"房地产","pid":"01","value":""]
]

var dataSouce: [MenuDataModel] = []
for dict in array {
    let dataModel = MenuDataModel.init(dict: dict)
    dataSouce.append(dataModel)
}
```
Show
* Swift

```swift
let menu = MultilevelStyle1Menu(title: "行业类型", dataSouce: dataSouce, completion:       { (resultString, model) in //'resultString' is combined with every level data that you have selected.'model' is the MenuDataModel that you have selected lastly.
    self.resultLabel.text = resultString
})
menu.show()
```
* Objective-C

```objective-c
__weak typeof(self) weakSelf = self;
MultilevelStyle1Menu *menu = [[MultilevelStyle1Menu alloc]initWithTitle:@"行业类型" dataSouce:dataSource option:nil customView:nil completion:^(NSString * text, MenuDataModel * model) {
    weakSelf.resultLabel.text = text;
}];
[menu show];
```

Important Property
```swfit
menu.allowSelectAnyLevelData = true 
// It's false by defalut that it means you must select the last level data, otherwise you can't confirm result.But If it's true, meaning that you can select any level data to confirm result.
```
![gif3](https://github.com/ChokShen/MultilevelMenu/raw/master/Screenshots/MultilevelStyle1Menu_True.gif)  
***MultilevelStyle1Menu***
 
![gif4](https://github.com/ChokShen/MultilevelMenu/raw/master/Screenshots/MultilevelStyle2Menu_True.gif)  
***MultilevelStyle2Menu***

## Custom Menu
* Custom Cell  
Creat your custom cell by xib or code.
* Create a MultilevelMenuStyle1View or MultilevelMenuStyle2View subclass
```swift
class CustomMenuView: MultilevelMenuStyle2View {
    // MARK: - UITableViewDelegate
    open override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        var cell = tableView.dequeueReusableCell(withIdentifier: "CustomCell") as? CustomCell
        if cell == nil {
            cell = Bundle.main.loadNibNamed("CustomCell", owner: nil, options: nil)?.first as? CustomCell
        }
        cell?.contenLabel?.text = dataSouce[indexPath.row].name //Text
        cell?.iconImageView.image = UIImage(named: dataSouce[indexPath.row].value!) //Image

        //Selected properties
        cell?.tintColor = option.checkMarkColor
        if indexPath == lastSelectedIndexPath {
            cell?.accessoryType = .checkmark
        } else {
            cell?.accessoryType = .none
        }
        return cell!
    }
}
```
![gif5](https://github.com/ChokShen/MultilevelMenu/raw/master/Screenshots/MultilevelStyle2Menu_Custom.gif)

*Show
* Swift

```swift
var option = MultilevelMenuOption()
option.rightBarButtonTitle = "ok"
option.rightBarButtonColor = UIColor.red

let menu = MultilevelStyle2Menu(title: "请选择行业类型", dataSouce: dataSouce, option: option, customView: CustomMenuView(), completion: { (resultString, model) in
    self.resultLabel.text = resultString
})
menu.show()
```
* Objective-C
```objective-c
MultilevelMenuOption *option = [[MultilevelMenuOption alloc]init];
option.rightBarButtonTitle = @"ok";
option.rightBarButtonColor = [UIColor redColor];
__weak typeof(self) weakSelf = self;
MultilevelStyle2Menu *menu = [[MultilevelStyle2Menu alloc] initWithTitle:@"请选择行业类型" fileUrl:url option:option customView:nil completion:^(NSString * text, MenuDataModel * model) {
    weakSelf.resultLabel.text = text;
}];
[menu show];
```
You also can creat MultilevelMenuOption to set menu custom properties.






