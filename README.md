# VIDUS iOS SDK
![version](https://img.shields.io/badge/version-v0.1.3-blue)


The Vidus SDK comes with a set of screens and configurations to record live video of customers. Each of the recording options in the SDK are called nodes which can be configured by developers.

- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)

## Features

- [x] Capture Video
- [x] Add nodes to customise Video flow
- [x] Compress recorded video

## Requirements

- Swift 5.0
- iOS 10.0+

<br>

## Installation
### Cocoapods

[CocoaPods](http://cocoapods.org) is a dependency manager for Cocoa projects.

To integrate **Vidus** into your Xcode project using CocoaPods, specify it in your `Podfile`:

```
source 'https://gitlab.com/frslabs-public/ios/vidus.git'
platform :ios, '10.0'
use_frameworks!
pod 'VIDUS', '0.1.3'
target '<Your Target Name>' do
end
```

Then, run the following command:

```bash
$ pod install
```

## Usage

### Swift

1. Make sure that you have created input parameter and node parameter:

```swift
class YourViewController: UIViewController {
    var inputParams = [[String : String]]()
    var nodeParams = [String : String]()
    
   override func viewDidLoad() {
      super.viewDidLoad()
      /// For simple node
      addNodeWithParam(nodeName: VIDUS.VidusNodeName.simpleRecorderNode.rawValue)
      /// For challenge text node
      addNodeWithParam(nodeName: VIDUS.VidusNodeName.challengeTextNode.rawValue)
      /// For challenge code node
      addNodeWithParam(nodeName: VIDUS.VidusNodeName.challengeCodeNode.rawValue)
      /// For declaration node
      addNodeWithParam(nodeName: VIDUS.VidusNodeName.declarationNode.rawValue)
      /// For osv recorder node
      addNodeWithParam(nodeName: VIDUS.VidusNodeName.oSVRecorderNode.rawValue)
      /// For osv challenge text node
      addNodeWithParam(nodeName: VIDUS.VidusNodeName.oSVChallengeTextNode.rawValue)
       /// For osv challenge text node
      addNodeWithParam(nodeName: VIDUS.VidusNodeName.oSVChallengeTextNode.rawValue)
      /// For Video with Custom Text
      addNodeWithParam(nodeName: VIDUS.VidusNodeName.videoWithCustomText.rawValue)
      /// Result
      addOutputObserver()
   }
}
```

2. Initialize Vidus framework:
```swift

/// - Parameters: input parameters
///   - results: The results of the user capturing video with the camera.
     override func viewDidAppear(_ animated: Bool) {
       if inputParam.count > 0{
         VIDUS.initialize(caller: self, node: inputParam)
       }
   }
   
   /// - Call following fuction on viewDidLoad for provide input to vidus framework
   
   func addNodeWithParam(nodeName:String) {
       switch nodeName {
       case VIDUS.VidusNodeName.simpleRecorderNode.rawValue:
            nodeParams[VIDUS.VidusParameter.nodeName.rawValue] = VIDUS.VidusNodeName.simpleRecorderNode.rawValue
            nodeParams[VIDUS.VidusParameter.timeDuration.rawValue] = "8"
            inputParams.append(nodeParams)
       case VIDUS.VidusNodeName.challengeCodeNode.rawValue:
            nodeParams[VIDUS.VidusParameter.nodeName.rawValue] = VIDUS.VidusNodeName.challengeCodeNode.rawValue
            nodeParams[VIDUS.VidusParameter.challengeCodeText.rawValue] = "Sample Text"
            inputParams.append(nodeParams)
       case VIDUS.VidusNodeName.challengeTextNode.rawValue:
            nodeParams[VIDUS.VidusParameter.nodeName.rawValue] = VIDUS.VidusNodeName.challengeTextNode.rawValue
            nodeParams[VIDUS.VidusParameter.challengeCodeText.rawValue] = "Sample Text"
            nodeParams[VIDUS.VidusParameter.timeDuration.rawValue] = "12"
            inputParams.append(nodeParams)
       case VIDUS.VidusNodeName.declarationNode.rawValue:
            nodeParams[VIDUS.VidusParameter.nodeName.rawValue] = VIDUS.VidusNodeName.declarationNode.rawValue
            nodeParams[VIDUS.VidusParameter.voiceType.rawValue] = VIDUS.VidusVoiceType.byMacine.rawValue
            nodeParams[VIDUS.VidusParameter.challengeCodeText.rawValue] = "Sample Text"
            inputParams.append(nodeParams)
       case VIDUS.VidusNodeName.oSVRecorderNode.rawValue:
            nodeParams[VIDUS.VidusParameter.nodeName.rawValue] = VIDUS.VidusNodeName.oSVRecorderNode.rawValue
            nodeParams[VIDUS.VidusParameter.timeDuration.rawValue] = "10"
            inputParams.append(nodeParams)
       case VIDUS.VidusNodeName.oSVChallengeTextNode.rawValue:
            nodeParams[VIDUS.VidusParameter.nodeName.rawValue] = VIDUS.VidusNodeName.oSVChallengeTextNode.rawValue
            nodeParams[VIDUS.VidusParameter.challengeCodeText.rawValue] = "Sample Text"
            nodeParams[VIDUS.VidusParameter.timeDuration.rawValue] = "13"
            inputParams.append(nodeParams)
        case VIDUS.VidusNodeName.oSVChallengeTextNode.rawValue:
        nodeParams[VIDUS.VidusParameter.nodeName.rawValue] = VIDUS.VidusNodeName.videoWithCustomText.rawValue
        nodeParams[VIDUS.VidusParameter.challengeCodeText.rawValue] = "Sample Text"
        inputParams.append(nodeParams)
       default:
           print("Error: Node is empty")
       }
   }

```

3. Finally, to get the output from Vidus framework, add the following function inside ViewController Class:

```swift
   func addOutputObserver(){
       NotificationCenter.default.addObserver(
           self,
           selector: #selector(self.output),
           name: NSNotification.Name(rawValue: NotificationName.output.description),
           object: nil)
      }
   @objc private func output(notification: NSNotification){
       if notification.object != nil{
           let result = notification.object as! SDKOutput
           let videoPath = result.videoPath
       }
   
```

## License

Vidus is available under the FRSLABS commercial license. Write to support@frslabs.com to get your Licence Key and Netrc credentials.

## Vidus Parameters

Vidus SDK has APIs to capture interactive realtime selfie video with customizable actions. Each functionality is separated into unique `nodes`. The APIs are contained in the following  input nodes.

1. **[Simple Recorder Node](#simple-recorder-node)**

2. **[Challenge Code Node](#challenge-code-node)**

3. **[Challenge Text Node](#challenge-text-node)**

4. **[Declaration Node](#declaration-node)** 

5. **[OSV Recorder Node](#osv-recorder-node)** 

6. **[OSV Challenge Text Node](#osv-challenge-text-node)**

7. **[Video With Custom Text](#Video-With-Custom-Text)**



The Input Nodes are explained below,

#### Simple Recorder Node

Captures a video recording with a user defined time.

<div>
<table style="width:100%">
  <tr>
    <th bgcolor="#F1F1F1" colspan="2">Public Methods</th>
  </tr>
  <tr>
    <td><b>nodeParameters[VIDUS.VidusParameter.timeDuration.rawValue] = </b>videoRecordTime</td>
    <td>Sets the time(<em>String</em>) taken for the recording of the node.
    </br></br> Values should be between <b> 1 and 100 </b> </td>
  </tr>
</table>
</div>

#### Challenge Code Node

Captures a video recording which will include a user-read 4 digit code prompted by a customizable machine-read question.

<div>
<table style="width:100%">
  <tr>
    <th bgcolor="#F1F1F1" colspan="2">Public Methods</th>
  </tr>
  <tr>
    <td><b>nodeParameters[VIDUS.VidusParameter.challengeCodeText.rawValue] = </b>videoChallengeCodeText</td>
    <td>Sets the text that will be spoken prior to displaying the code.</td>
  </tr>
  <tr>
 </tr>
</table>
</div>


#### Challenge Text Node

Captures a video recording which will include a user-prompted answer with a customizable machine-read question.

<div>
<table style="width:100%">
 <tr>
 <th bgcolor="#F1F1F1" colspan="2">Public Methods</th>
 </tr>
 <tr>
 <td><b>nodeParameters[VIDUS.VidusParameter.challengeCodeText.rawValue] = </b>videoChallengeText</td>
 <td>Sets the text that will be spoken.</td>
 </tr>
 <tr>
 <td><b>nodeParameters[VIDUS.VidusParameter.timeDuration.rawValue]</b> videoChallengeTextTime</td>
 <td>Sets the recording time(<em></em>) for the node after the text is spoken.
 </br></br> Values should be between <b> 1 and 100 </b> </td>
 </tr>
 <tr>
 </tr>
</table>
</div>

#### Declaration Node

Captures a video recording which will include a user-read or machine-read text/declaration.

<div>
<table style="width:100%">
 <tr>
 <th bgcolor="#F1F1F1" colspan="2">Public Methods</th>
 </tr>
 <tr>
 <td><b> nodeParameters[VIDUS.VidusParameter.challengeCodeText.rawValue] = </b>videoDeclarationText</td>
 <td>Sets the text that will be displayed.</td>
 </tr>
 <tr>
</td>
 </tr>
 <tr>
 <td><b>nodeParameters[VIDUS.VidusParameter.voiceType.rawValue] = </b>(<em>int videoDeclarationSpokenMethod</em>)</td>
 <td>(OPTIONAL) </br> Sets the way the text is to be spoken, 
 </br> Either by the user or by the machine.</br>
 </br> Values are </br> <b>VIDUS.VidusVoiceType.byMacine.rawValue </br> VIDUS.VidusVoiceType.byUser.rawValue </b>(Default Value) </td>
 </tr>
</table>
</div>

#### OSV Recorder Node

Captures a video recording with a user defined time.

<div>
<table style="width:100%">
<tr>
<th bgcolor="#F1F1F1" colspan="2">Public Methods</th>
</tr>
<tr>
<td><b> nodeParameters[VIDUS.VidusParameter.timeDuration.rawValue] = </b>videoRecordTime</td>
<td>Sets the time(<em></em>) taken for the recording of the node.
</br></br> Values should be between <b> 1 and 100 </b> </td>
</tr>
</table>
</div>

#### OSV Challenge Text Node

<div>
<table style="width:100%">
 <tr>
 <th bgcolor="#F1F1F1" colspan="2">Public Methods</th>
 </tr>
 <tr>
 <td><b>nodeParameters[VIDUS.VidusParameter.challengeCodeText.rawValue] = </b>videoChallengeText</td>
 <td>Sets the text that will be spoken.</td>
 </tr>
 <tr>
 <td><b>nodeParameters[VIDUS.VidusParameter.timeDuration.rawValue]</b> videoChallengeTextTime</td>
 <td>Sets the recording time(<em></em>) for the node after the text is spoken.
 </br></br> Values should be between <b> 1 and 100 </b> </td>
 </tr>
 <tr>
 </tr>
</table>
</div>


#### Video With Custom Text

<div>
<table style="width:100%">
 <tr>
 <th bgcolor="#F1F1F1" colspan="2">Public Methods</th>
 </tr>
 <tr>
 <td><b>nodeParameters[VIDUS.VidusParameter.challengeCodeText.rawValue] = </b>videoChallengeText</td>
 <td>Sets the text that will be spoken.</td>
 </tr>
 <tr>
 </tr>
 <tr>
 </tr>
</table>
</div>



