# VIDUS iOS SDK
![version](https://img.shields.io/badge/version-v0.1.4-blue)


The Vidus SDK comes with a set of screens and configurations to record live video of customers. Each of the recording options in the SDK are called nodes which can be configured by developers.

You can find the release history at [Changelog](CHANGELOG.md)

# Table Of Content

- [Prerequisite](#prerequisite)
- [Requirements](#requirements)
- [Installation](#installation)
- [Getting Started](#getting-started)
- [Vidus Parameters](#vidus-parameters)
- [Vidus Error Codes](#vidus-error-codes)
- [Help](#help)

## Prerequisite

You will need a valid license and Netrc credentials to use the Vidus SDK, which can be obtained by contacting support@frslabs.com .

Depending on the license - offline or online - you have opted for, the ping functionality to billing servers will be disabled or enabled. For instance, if you have opted for the offline SDK model, then there will be no server ping needed to our billing server to bill you. However, if you have chosen a transaction based pricing, then after each transaction, a ping request will be made to our billing server. This cannot be overrided by the App. A point to note is that if the ping transaction fails for any reason, the whole transaction will be void without any results from the SDK.

Once you have the license , follow the below instructions for a successful integration of Vidus SDK onto your iOS Application.

## Requirements

- Swift 5.0
- iOS 10.0+

## Installation

### Cocoapods

To integrate **Vidus** into your Xcode project using CocoaPods, specify it in your `Podfile`:

```
source 'https://gitlab.com/frslabs-public/ios/vidus.git'
platform :ios, '10.0'
use_frameworks!
pod 'VIDUS', '0.1.4'
pod 'Alamofire', '~> 4.8.2-rc.3'
target '<Your Target Name>' do
end
```

Then, run the following command:

```bash
$ pod install
```

## Getting Started

### Swift

1. Initialize the input parameters including the `nodes` to be invoked

```swift
class YourViewController: UIViewController {

    var inputParams = [[String : String]]()
    var nodeParameters = [String : String]()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // For simple node
        addNodeWithParam(nodeName: VIDUS.VidusNodeName.simpleRecorderNode.rawValue)
        
        // For challenge text node
        addNodeWithParam(nodeName: VIDUS.VidusNodeName.challengeTextNode.rawValue)
        
        // For challenge code node
        addNodeWithParam(nodeName: VIDUS.VidusNodeName.challengeCodeNode.rawValue)
        
        // For declaration node
        addNodeWithParam(nodeName: VIDUS.VidusNodeName.declarationNode.rawValue)
        
        // For osv recorder node
        addNodeWithParam(nodeName: VIDUS.VidusNodeName.oSVRecorderNode.rawValue)
        
        // For osv challenge text node
        addNodeWithParam(nodeName: VIDUS.VidusNodeName.oSVChallengeTextNode.rawValue)
        
        // For osv challenge text node
        addNodeWithParam(nodeName: VIDUS.VidusNodeName.oSVChallengeTextNode.rawValue)
        
        // For Video with Custom Text
        addNodeWithParam(nodeName: VIDUS.VidusNodeName.PIVNode.rawValue)
        
        // Result
        addOutputObserver()
    }
    
    // ...
}
```

2. Invoke Vidus SDK

```swift
    // ...
    
    override func viewDidAppear(_ animated: Bool) {
        if inputParams.count > 0{
           VIDUS.initialize(caller: self, nodeParam: inputParams, licenceKey: "Enter licence Key")
        }
    }
    
    // ...    
```

3. Call following fuction on viewDidLoad for provide input to vidus framework
   
```swift
    // ...
    
    func addNodeWithParam(nodeName:String) {
        switch nodeName {
        case VIDUS.VidusNodeName.simpleRecorderNode.rawValue:
            nodeParameters[VIDUS.VidusParameter.nodeName.rawValue] = VIDUS.VidusNodeName.simpleRecorderNode.rawValue
            nodeParameters[VIDUS.VidusParameter.timeDuration.rawValue] = "8"
            inputParams.append(nodeParameters)
        case VIDUS.VidusNodeName.challengeCodeNode.rawValue:
            nodeParameters[VIDUS.VidusParameter.nodeName.rawValue] = VIDUS.VidusNodeName.challengeCodeNode.rawValue
            nodeParameters[VIDUS.VidusParameter.challengeCodeText.rawValue] = "Sample Text"
            nodeParameters[VIDUS.VidusParameter.baseUrl.rawValue] = "BASE_URL"
            nodeParameters[VIDUS.VidusParameter.keyId.rawValue] = "KEY_ID"
            nodeParameters[VIDUS.VidusParameter.keySecret.rawValue] = "KEY_SECRET" 
            inputParams.append(nodeParameters)
        case VIDUS.VidusNodeName.challengeTextNode.rawValue:
            nodeParameters[VIDUS.VidusParameter.nodeName.rawValue] = VIDUS.VidusNodeName.challengeTextNode.rawValue
            nodeParameters[VIDUS.VidusParameter.challengeCodeText.rawValue] = "Sample Text"
            nodeParameters[VIDUS.VidusParameter.timeDuration.rawValue] = "12"
            inputParams.append(nodeParameters)
        case VIDUS.VidusNodeName.declarationNode.rawValue:
            nodeParameters[VIDUS.VidusParameter.nodeName.rawValue] = VIDUS.VidusNodeName.declarationNode.rawValue
            nodeParameters[VIDUS.VidusParameter.voiceType.rawValue] = VIDUS.VidusVoiceType.byMacine.rawValue
            nodeParameters[VIDUS.VidusParameter.challengeCodeText.rawValue] = "Sample Text"
            inputParams.append(nodeParameters)
        case VIDUS.VidusNodeName.oSVRecorderNode.rawValue:
            nodeParameters[VIDUS.VidusParameter.nodeName.rawValue] = VIDUS.VidusNodeName.oSVRecorderNode.rawValue
            nodeParameters[VIDUS.VidusParameter.timeDuration.rawValue] = "10"
            inputParams.append(nodeParameters)
        case VIDUS.VidusNodeName.oSVChallengeTextNode.rawValue:
            nodeParameters[VIDUS.VidusParameter.nodeName.rawValue] = VIDUS.VidusNodeName.oSVChallengeTextNode.rawValue
            nodeParameters[VIDUS.VidusParameter.challengeCodeText.rawValue] = "Sample Text"
            nodeParameters[VIDUS.VidusParameter.timeDuration.rawValue] = "13"
            inputParams.append(nodeParameters)
        case VIDUS.VidusNodeName.PIVNode.rawValue:
            nodeParameters[VIDUS.VidusParameter.nodeName.rawValue] = VIDUS.VidusNodeName.PIVNode.rawValue
            nodeParameters[VIDUS.VidusParameter.challengeCodeText.rawValue] = "Sample Text"
            nodeParameters[VIDUS.VidusParameter.baseUrl.rawValue] = "BASE_URL"
            nodeParameters[VIDUS.VidusParameter.keyId.rawValue] = "KEY_iD"
            nodeParameters[VIDUS.VidusParameter.keySecret.rawValue] = "KEY_SECRET"  
            inputParams.append(nodeParameters)
        default:
            print("Error: Node is empty")
        }
    }
    
    // ...
```

4. Finally, to get the output from Vidus framework, add the following function inside ViewController Class:

```swift
    // ...
     
    func addOutputObserver(){
        NotificationCenter.default.addObserver(
            self,
            selector: #selector(self.output),
            name: NSNotification.Name(rawValue: NotificationName.output.rawValue),
            object: nil)
    }
    
    @objc private func output(notification: NSNotification){
        if notification.object != nil{
            let objectName = notification.object as! NodeCollectionController
            let outputURL = objectName.VideoSDKResultURL
            let ErrorCode = objectName.ErrorCode
            let secretCodeVerificationResponse = objectName.SecretNumberVerificationStatus
        }
    }
    
    // ...
```

## Vidus Parameters

Vidus SDK has APIs to capture interactive realtime selfie video with customizable actions. Each functionality is separated into unique `nodes`. The APIs are contained in the following  input nodes.

1. **[Simple Recorder Node](#simple-recorder-node)**

2. **[Challenge Code Node](#challenge-code-node)**

3. **[Challenge Text Node](#challenge-text-node)**

4. **[Declaration Node](#declaration-node)** 

5. **[OSV Recorder Node](#osv-recorder-node)** 

6. **[OSV Challenge Text Node](#osv-challenge-text-node)**

7. **[PIV Node](#piv-node)** (Pre-Issuance Verification Node)


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
 <td><b>nodeParameters[VIDUS.VidusParameter.timeDuration.rawValue] = </b> videoChallengeTextTime</td>
 <td>Sets the recording time(<em></em>) for the node after the text is spoken.
 </br></br> Values should be between <b> 1 and 100 </b> </td>
 </tr>
</table>
</div>


#### PIV Node

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
 <td><b>nodeParameters[VIDUS.VidusParameter.baseUrl.rawValue] = </b>videoApiBaseUrl</td>
 <td>Vidus API Base URL.
 </tr>
</table>
</div>


## Vidus Error Codes

Following error codes will be returned on the `onVidusFailure` method of the callback

| CODE | DESCRIPTION                  |
| ---- | ---------------------------- |
| 803  | Camera or Microphone Permission denied             |
| 804  | SDK was interrupted       |
| 805  | Vidus SDK License expired            |
| 806  | Vidus SDK License was invalid |
| 807  | Invalid Config         |
| 808  | Transaction Failed       |
| 809  | No Internet Available             |
| 810  | Screen Recording Permision denied             |
| 811  | Upload Video to server failed            |
| 812  | Upload Audio to server failed            |
| 813  | Upload Image to server failed            |

## Help
For any queries/feedback , contact us at `support@frslabs.com` 

