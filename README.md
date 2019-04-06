# VeriView™ SDK

To use our library follow this simple steps:

1.  We use CocoaPods as a dependency manager, our pod is located in a private repository and you can integrate it to your project like this:
```
#Podfile example
use_frameworks!

source 'https://gitlab.com/jinglz/apps/ios-cocoapods-specs.git'
source 'https://github.com/CocoaPods/Specs.git'

target 'YourApp' do
pod 'VeriViewSDK'
end
```

2. You need to add Camera Privacy settings to your plist: 
```
<key>NSCameraUsageDescription</key>
<string>We need your camera for VeriView™</string>
```

3.  Set your controller as a **VeriViewController'** and as **VeriViewDelegate**, don't forget to import the library:

```swift
import VideoCaptureSDK

class VideoViewController: VeriViewController, , VeriViewDelegate {
    ...
}
```

You'll need to implement this methods, were you can put your code:

```swift
func didEngaged()
func didNotEngaged()
func volumeFull()
func volumeDown()
func loadedWithStatus(state: VeriViewStatus)
```

In **loadedWithStatus** you would know the status in which VeriView™ loaded, the status are:

```swift
enum VeriViewStatus{
    case ERROR_LOADING
    case NO_PAUSE
    case NO_EVENTS
    case NO_PAUSE_NO_EVENTS
    case FULL_VERIFICATION
}
```

4.  In viewDidLoad call you can begin VeriView™ capture and set your keys, like this:

```swift
override public func viewDidLoad() {
    self.beginCapture(apiKey: "yourApiKey", resourceId: "yourResourceId", reproductionId: "yourReproductionId"){(res) in
        if res{
            self.startCamera()
        }
        if !self.pauseVideo{
            self.player?.play()
        }
    }
...
```

5. If you want your emotions and events to be tracked by seconds you need to implement a timer or a way to count those seconds in your code, and update **self.second**. Otherwise everything will be send with second = 0.

6. You can send events (if they're in your settings) like this:

```swift
    self.manager.sendEvent(event: .SKIP_VIDEO, resourceId: "yourResourceId", reproductionId: "yourReproduction", apiKey: "yourApiKey", time: String(self.second))
```

The type of events that you cand send are:

```swift
enum Event: String{
    case NOT_ENGAGED = "NOT_ENGAGED"
    case ENGAGED = "ENGAGED"
    case VOLUME_DOWN = "VOLUME_DOWN"
    case VOLUME_UP = "VOLUME_UP"
    case SURVEY_YES = "SUVEY_YES"
    case SURVEY_NO = "SURVEY_NO"
    case SKIP_VIDEO = "SKIP_VIDEO"
    case COMPLETED_VIDEO = "COMPLETED_VIDEO"
    case CLICK_CTA = "CLICK_CTA"
    case PAUSED_BY_VOL = "PAUSED_BY_VOL"
    case RESUMED_BY_VOL = "RESUMED_BY_VOL"
}
```

7.  When you want to stop VeriView™ capture, just call `self.stopCamera()`


