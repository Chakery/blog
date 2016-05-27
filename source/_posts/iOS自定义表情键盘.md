---
title: iOS自定义表情键盘
date: 2016-05-27
---

[GitHub下载](https://github.com/Chakery/EmojiKeyBoard)
### 废话
在做表情键盘时，很多时候为了使得各个平台的表情得到统一(或者是对表情的扩展等等)，因此采用自定义的图片表情，而非系统自带的表情。
    前段时间刚好遇到这样的需求, 因此打算动手写一个(造轮子)， 粗糙的轮子成形之后，决定从以下几个方面入手，做点小笔记。

* [表情建模](#0)
* [自定义图片表情的排版](#1)
* [在UITextView中插入表情](#2)
* [表情转换字符串](#3)
* [字符串转换表情](#4)

先上图 :
![](https://raw.githubusercontent.com/Chakery/images/master/emojiKeyBoard/1.png)
![](https://raw.githubusercontent.com/Chakery/images/master/emojiKeyBoard/2.png)
![](https://raw.githubusercontent.com/Chakery/images/master/emojiKeyBoard/3.png)

----------

### 正文
<h3 id="0">1. 表情建模</h3>
表情建模，个人感觉是非常非常有必要的(毕竟一个表情包含的不仅仅是图片)，表情大概具有以下属性：
1) ID (对应的表情包id)
2) image (对应的图片)
3) code (十六进制编码, 如:系统表情)
4) isDelete (是不是删除按钮)
当然，还有表情包的模型，不再赘述。

<h3 id="1">2. 自定义图片表情的排版</h3>
表情布局方式很多，比如：UIScrollView，UICollectionView等等，这里选择后者，别问为什么，两个字： ***简单***。
***但是：在设置了`scrollDirection = .Horizontal`之后，UICollectionViewCell的布局就变成了`从上到下,从左到右`的形式。如图：***
![](https://raw.githubusercontent.com/Chakery/images/master/emojiKeyBoard/1.1.png)
这样就会导致，在分页滚动时, 最后一页可能出现半页的情况，既然已经知道了问题是怎么导致的，那么解决这样的问题也就简单了，只需要重写 `UICollectionViewLayout` 给 `UICollectionViewCell` 重新布局便可。

代码如下：
```swift
class EmojiLayout: UICollectionViewLayout {
	// 保存所有item属性
	private var attributes: [UICollectionViewLayoutAttributes] = []
	// screen
	private let mainRect = UIScreen.mainScreen().bounds
	// section
	private var sections: Int = 0
	// item
	private var items: Int = 0
	// column
	var maxColumn: CGFloat = 0
	// row
	var maxRow: CGFloat = 0
	// margin
	var margin: CGFloat = 0

	// MARK: - 允许重新布局
	override func shouldInvalidateLayoutForBoundsChange(newBounds: CGRect) -> Bool {
		return true
	}

	// MARK: - 重新布局
	override func prepareLayout() {
		super.prepareLayout()
        
		attributes.removeAll()
        
        // 根据设置的Column Row, 计算得到每个item的大小
		let itemsize = getItemSize(maxColumn, row: maxRow, margin: margin)
		// 获取组数
		sections = self.collectionView?.numberOfSections() ?? 0
		// 遍历每组里面的所有item
		for section in 0 ..< sections {
			items = self.collectionView?.numberOfItemsInSection(section) ?? 0
			// 遍历每一个item
			for item in 0 ..< items {
			    // 根据 section, item 获取每一个item的indexPath值
				let indexPath = NSIndexPath(forItem: item, inSection: section)
				// 根据indexPath值, 获取每一个item的属性
				let attribute = UICollectionViewLayoutAttributes(forCellWithIndexPath: indexPath)
                // 通过一系列脑残计算, 得到x, y值
				let x = margin + (itemsize.width + margin) * (CGFloat(item) % maxColumn) + (CGFloat(section) * mainRect.width)
				let y = margin + (itemsize.height + margin) * CGFloat(item / Int(maxColumn))
				attribute.frame = CGRect(x: x, y: y, width: itemsize.width, height: itemsize.height)
                // 把每一个新的属性保存起来
				attributes.append(attribute)
			}
		}
	}

	// MARK: - 返回当前可见的
	override func layoutAttributesForElementsInRect(rect: CGRect) -> [UICollectionViewLayoutAttributes]? {
		var rectAttributes: [UICollectionViewLayoutAttributes] = []
		// 遍历所有属性, 返回当前处于可见区域的item(在屏幕可见的item)
		let _ = attributes.map {
			if CGRectContainsRect(rect, $0.frame) {
				rectAttributes.append($0)
			}
		}
		return rectAttributes
	}

	// MARK: - 返回大小
	override func collectionViewContentSize() -> CGSize {
		let itemsize = getItemSize(maxColumn, row: maxRow, margin: margin)
		return CGSize(width: CGFloat(sections) * mainRect.width, height: margin + (maxRow * (itemsize.height + margin)))
	}

	// MARK: - itemSize
	// 为了使得表情不变形, 因此 height = width
	private func getItemSize(column: CGFloat, row: CGFloat, margin: CGFloat) -> CGSize {
		let width = (mainRect.width - ((column + 1) * margin)) / column
		return CGSize(width: width, height: width)
	}
}
```
最后结果，如图：
![](https://raw.githubusercontent.com/Chakery/images/master/emojiKeyBoard/1.2.png)
没错，就是这么简单。
表情展示的时候，本人的做法：表情页(每个section)，通过计算得到，表情包能分成多少个section。切换表情包的时候，修改collectionView的datasource，然后reloadData
当然，做法不是唯一的。

<h3 id="2">3. 在UITextView中插入表情</h3>
在`UITextView`中插入表情，分为2种情况：`系统表情` 和 `图片表情`
1）系统表情
系统表情，其实就是字符串（个人感觉是字符串，不懂这样理解对否），因此不需要我们做太多的操作，可以直接给`UITextView`的`text`属性赋值。
2）图片表情
图片表情插入到 `UITextView`，可以通过 `attributedText` 属性进行设置(这几乎是最简单的方式法了)
主要2点：
a. 为了使用方便，直接给 `UITextView` 扩展方法 `func insertEmoji(emojiModel: EmojiModel)`
b. 继承 `NSTextAttachment` 添加一个属性 `emojiTag` 用于记录该表情对应的字符串

具体操作如下：
```swift
extension UITextView {
	/// 插入表情
	///
	/// - parameter emojiModel: 表情模型
	func insertEmoji(emojiModel: EmojiModel) {
		// 删除按钮
		if emojiModel.deleteBtn {
			deleteBackward()
			return
		}
		// 系统表情
		if let emoji = emojiModel.emoji {
			insertText(emoji)
		}
		// png 表情
		if let pngImage = emojiModel.pngImage {
			// 创建附件
			let attachment = EmojiTextAttachment()
			// 设置图片
			attachment.image = pngImage
			// 设置图片标志（这里设置图片标志，主要是为了：表情转换字符串时 操作更简单）
			attachment.emojiTag = emojiModel.chs
			// 设置附件大小 (表情跟文本的大小一致)
			attachment.bounds = CGRect(x: 0, y: -4, width: font!.lineHeight, height: font!.lineHeight)

			// 带属性的文本, 把图片设置进去
			let attritubeString = NSMutableAttributedString(attributedString: NSAttributedString(attachment: attachment))
			// 设置字体
			attritubeString.addAttribute(NSFontAttributeName, value: font!, range: NSRange(location: 0, length: 1))

			// 获取原来的文本, 替换为现在的文本, 再给textView的属性文本赋值
			let att = NSMutableAttributedString(attributedString: attributedText)
			att.replaceCharactersInRange(self.selectedRange, withAttributedString: attritubeString)
			attributedText = att

			// 移动光标
			selectedRange.location += 1

			// 重写通知, 代理方法
			NSNotificationCenter.defaultCenter().postNotificationName(UITextViewTextDidChangeNotification, object: self)
			delegate?.textViewDidChange!(self)
		}
	}
}
```

<h3 id="3">4. 表情转换字符串</h3>
由于服务器无法识别 `NSAttributedString` 这样的属性文本，因此有必要把表情转换成对应的字符串。
既然图片表情是通过 `NSAttributedString` 进行设置的，那么同样的道理，我们可以再通过遍历属性，找到图片表情对应的字符串。

如下：
```swift
extension NSAttributedString {
	// 遍历属性, 获取字符串
	func getPlainString() -> String {
		let plainStr = NSMutableString(string: self.string)
		var base: Int = 0
		self.enumerateAttribute(NSAttachmentAttributeName, inRange: NSMakeRange(0, self.length), options: []) { (value, range, stop) -> Void in
			if let value = value as? EmojiTextAttachment {
				if let emojiTag = value.emojiTag {
					plainStr.replaceCharactersInRange(NSMakeRange(range.location + base, range.length), withString: emojiTag)
					base += emojiTag.characters.count - 1
				}
			}
		}
		return plainStr as String
	}
}
```

<h3 id="4">5. 字符串转换表情</h3>
最后，可能有人觉得很奇怪，为什么要把表情转换成字符串，然后字符串又要转换成表情？
其实，这两者并不冲突，也不是多此一举，而是很有必要的操作，为啥呢？
如图：
![](https://raw.githubusercontent.com/Chakery/images/master/emojiKeyBoard/1.3.png)

字符串转换表情，本人的做法很low，并不建议这么做，倘若你也没有什么办法的话，那就勉强看一下吧（前提是，你的表情对应的字符串是这样的：`[哈哈] [玫瑰] [爱心]`）。
直接上代码：
```swift
/// 符合"标准"的字符串
struct StringProperty {
	/// 字符串
	var value: String
	/// 位置
	var range: NSRange

	init(value: String, range: NSRange) {
		self.value = value
		self.range = range
	}
}

extension String {
	/// 字符串查找, 返回以start开始 且 以end结尾的字符串
	///
	/// - parameter start: 开始字符串
	/// - parameter end:   结束字符串
	///
	/// - returns: 如果存在, 返回 StringProperty 对象, 否则返回nil
	func between(start: String, _ end: String) -> StringProperty? {
		var range = NSRange()
		var flag: Bool = false
		let string = self as NSString

		for var i = 0; i < string.length; i += 1 {
			let subStr = string.substringWithRange(NSRange(location: i, length: 1))
			if start == subStr {
				range.location = i
				flag = true
				continue
			} else if flag && subStr == end {
				flag = false
				range.length = i - range.location - 1
				let strRange = NSRange(location: range.location, length: range.length + start.characters.count + end.characters.count)
				let value = string.substringWithRange(strRange)
				return StringProperty(value: value, range: strRange)
			}
		}
		return nil
	}
}
```
通过`func between(start: String, _ end: String) -> StringProperty?`方法可以获取到“有可能是表情的字符串”，接下来就是要确定这个字符串是不是表情。本人也是用了很low的办 —— “遍历表情包”
具体做法，可以看demo中的[EmojiPackageManager.swift](https://github.com/Chakery/EmojiKeyBoard/blob/master/EmojiKeyBoard%2FEmojiPackageManager.swift)文件下的`class func verificationEmojiWithString(string: String) -> EmojiModel?`方法。
