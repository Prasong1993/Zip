![Zip - Zip และ Unzip ไฟล์ใน Swift](https://cloud.githubusercontent.com/assets/889949/12374908/252373d0-bcac-11e5-8ece-6933aeae8222.png)

# Zip
เฟรมเวิร์ก Swift สำหรับการบีบอัดและคลายการบีบอัดไฟล์ ใช้งานง่ายและรวดเร็ว สร้างบนพื้นฐาน minizip

## การใช้งาน

นำเข้า Zip ที่ด้านบนของไฟล์ Swift

```swift
import Zip
```

## ฟังก์ชันด่วน

วิธีง่ายที่สุดในการใช้ Zip คือผ่านฟังก์ชันด่วน ทั้งสองใช้เส้นทางไฟล์ในเครื่องเป็น NSURLs โยนหากพบข้อผิดพลาดและส่งกลับ NSURL ไปยังปลายทางหากสำเร็จ
```swift
do {
    let filePath = Bundle.main.url(forResource: "file", withExtension: "zip")!
    let unzipDirectory = try Zip.quickUnzipFile(filePath) // คลายการบีบอัด
    let zipFilePath = try Zip.quickZipFiles([filePath], fileName: "archive") // บีบอัด
}
catch {
  print("มีบางอย่างผิดพลาด")
}
```

## การบีบอัดขั้นสูง

สำหรับการใช้งานขั้นสูง Zip มีฟังก์ชันที่ให้คุณตั้งเส้นทางปลายทางแบบกำหนดเอง ทำงานกับไฟล์ zip ที่มีการป้องกันด้วยรหัสผ่าน และใช้การปิดการจัดการความคืบหน้า ฟังก์ชันเหล่านี้จะโยนหากมีข้อผิดพลาด

```swift
do {
    let filePath = Bundle.main.url(forResource: "file", withExtension: "zip")!
    let documentsDirectory = FileManager.default.urls(for:.documentDirectory, in: .userDomainMask)[0]
    try Zip.unzipFile(filePath, destination: documentsDirectory, overwrite: true, password: "password", progress: { (progress) -> () in
        print(progress)
    }) // คลายการบีบอัด

    let zipFilePath = documentsFolder.appendingPathComponent("archive.zip")
    try Zip.zipFiles([filePath], zipFilePath: zipFilePath, password: "password", progress: { (progress) -> () in
        print(progress)
    }) // บีบอัด

}
catch {
  print("มีบางอย่างผิดพลาด")
}
```

## ส่วนขยายไฟล์แบบกำหนดเอง

Zip รองรับไฟล์ '.zip' และ '.cbz' โดยค่าเริ่มต้น เพื่อรองรับส่วนขยายไฟล์ zip ที่ได้มาเพิ่มเติม:
```swift
Zip.addCustomFileExtension("file-extension-here")
```

### [ที่นิยมใช้] การตั้งค่าด้วย Swift Package Manager
หากต้องการใช้ Zip กับ Swift Package Manager ให้เพิ่มลงในการพึ่งพาของแพคเกจของคุณ:
```swift
.package(url: "https://github.com/marmelroy/Zip.git", .upToNextMinor(from: "2.1"))
```

### การตั้งค่าด้วย CocoaPods
```ruby
source 'https://github.com/CocoaPods/Specs.git'
pod 'Zip', '~> 2.1'
```

### การตั้งค่าด้วย Carthage
หากต้องการรวม Zip เข้าไปในโปรเจ็กต์ Xcode ของคุณโดยใช้ Carthage ให้ระบุใน Cartfile ของคุณ:

```ogdl
github "marmelroy/Zip" ~> 2.1
```