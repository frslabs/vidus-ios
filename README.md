# VIDUS iOS SDK
![version](https://img.shields.io/badge/version-v2.1.0-blue)


The Vidus SDK comes with a set of screens and configurations to record live video of customers. Each of the recording options in the SDK are called nodes which can be configured by developers.Vidus is supporting English and Hindi languages.

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

You will need a valid license and Netrc credentials to use the Vidus SDK, which can be obtained by contacting support@frslabs.com.

Depending on the license - offline or online - you have opted for, the ping functionality to billing servers will be disabled or enabled. For instance, if you have opted for the offline SDK model, then there will be no server ping needed to our billing server to bill you. However, if you have chosen a transaction based pricing, then after each transaction, a ping request will be made to our billing server. This cannot be overrided by the App. A point to note is that if the ping transaction fails for any reason, the whole transaction will be void without any results from the SDK.

Once you have the license , follow the below instructions for a successful integration of Vidus SDK onto your iOS Application.

## Requirements

- Swift 5.0
- iOS 11.0+

## Permission

In Info.plist file add following code to allow your application to access iPhone's camera and microphone:
``<key>NSCameraUsageDescription</key>
<string>Allow access to camera</string>``

``<key>NSMicrophoneUsageDescription</key>
<string>Allow access to microphone</string>``

## Installation

### Cocoapods

To integrate **Vidus** into your Xcode project using CocoaPods, specify it in your `Podfile`:

```
source 'https://gitlab.com/frslabs-public/ios/vidus.git'
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '12.0'
target '<Your Target Name>' do
use_frameworks!
pod 'VIDUS', '2.1.0'
pod 'Alamofire', '~> 4.9.1'
end
```

Then, run the following command:

```bash
$ pod install
```

## Getting Started

### Swift

1. Initialize the input parameters including the `nodes` to be invoked and import delegate RecordingDelegate

```swift
class YourViewController: UIViewController,RecordingDelegate {

   func screenRecording(recording: ScreenNavigationViewController, didFinishRecordingWithResult results: VidusResults) {
       // Success Response
      let videoUrl = results.vidusVideoUrl
      let secretNumberValidationResponse = results.SecretNumberVerificationStatus
      let nodeReferenceData = results.nodeReferenceData
      let imageReferenceId  = results.imageReferenceId
   }
   func screenRecording(recording: ScreenNavigationViewController, didFailWithError error: String) {
        // Get Error
        let errorCode = error
   }

   var inputParam = [[String : String]]()
   var inputParameters = [String : String]()
   var nodeNameArray = [String]()
    
   override func viewDidLoad() {
        super.viewDidLoad()
      
      // MARK:  For Simple Node
       
       addNodeWithParam(nodeName: VidusInput.VidusInputNode.simpleRecorderNode.rawValue, timeDuration: "10", baseUrl: nil,      keyId: nil, keySecret: nil, messageText: nil, voiceType: nil, questionID: nil,languageCode: "", questionTemplatePath: "")
     
      // MARK:  For Challenge Code Node
       
       addNodeWithParam(nodeName: VidusInput.VidusInputNode.challengeCodeNode.rawValue, timeDuration: nil, baseUrl: "Base_URL", keyId: "KEY_ID", keySecret: "KEY_SECRET", messageText: "Sample_Text", voiceType: nil, questionID: nil,languageCode: "", questionTemplatePath: "")
        
      // MARK:  For Challenge Text Node
        
       addNodeWithParam(nodeName: VidusInput.VidusInputNode.challengeTextNode.rawValue, timeDuration: "10", baseUrl: nil, keyId: nil, keySecret: nil, messageText: "Sample Text", voiceType: nil, questionID: nil,languageCode: "", questionTemplatePath: "")
        
      // MARK:  For Declaration Node
        
       addNodeWithParam(nodeName: VidusInput.VidusInputNode.declarationNode.rawValue, timeDuration: nil, baseUrl: nil, keyId: nil, keySecret: nil, messageText: "Sample Text", voiceType: VidusInput.VoiceType.byMacine.rawValue, questionID: nil,languageCode: "", questionTemplatePath: "")
        
      // MARK:  For OSV Recorder Node
        
       addNodeWithParam(nodeName: VidusInput.VidusInputNode.oSVRecorderNode.rawValue, timeDuration: "10", baseUrl: nil, keyId: nil, keySecret: nil, messageText: nil, voiceType: nil, questionID: nil,languageCode: "", questionTemplatePath: "")
        
      // MARK:  For OSV Challenge Node
        
       addNodeWithParam(nodeName: VidusInput.VidusInputNode.oSVChallengeTextNode.rawValue, timeDuration: "10", baseUrl: nil, keyId: nil, keySecret: nil, messageText: "sample text", voiceType: nil, questionID: nil,languageCode: "", questionTemplatePath: "")
       
     // MARK: For PIV Node
    
       addNodeWithParam(nodeName: VidusInput.VidusInputNode.PIVNode.rawValue, timeDuration: nil, baseUrl: nil, keyId: "Enter Key ID", keySecret: "Enter Key Secret", messageText: "Enter Text", voiceType: nil, questionID: "Enter Question ID",languageCode: "", questionTemplatePath: "")
       
     // MARK: For PIVC Node
    
      addNodeWithParam(nodeName: VidusInput.VidusInputNode.PIVCNode.rawValue, timeDuration: nil, baseUrl: nil, keyId: nil, keySecret: nil, messageText: nil, voiceType: nil, questionID: nil, languageCode: "ENTER LANGUAGE CODE", questionTemplatePath: "ENTER QUESTION JSON STRING")
       
          
    }
    
    // ...
}
```

2. Invoke Vidus SDK

```swift
    // ...
    
    override func viewDidAppear(_ animated: Bool) {
         if inputParam.count > 0{
           let recorder = ScreenNavigationViewController(delegate: self)
           recorder.modalPresentationStyle = .fullScreen
           recorder.recordingNodeName = nodeNameArray
           recorder.nodeData = inputParam
           recorder.recordingType = VidusInput.RecordingType.screen.rawValue
           recorder.licenceKey = "ENTER LICENCE KEY"
           recorder.serverHeader = "ENTER SERVER_HEADER"
           recorder.referenceID = "ENTER_SERVER_REFERENCE_ID"
           recorder.isEnableInstructionScreen = false/true
           recorder.isEnablePreviewScreen = false/true
           present(recorder, animated: true)
           inputParam.removeAll()
         }
    }
    
    // ...    
```

3. Call following fuction on viewDidLoad for provide input to vidus framework
   
```swift
    // ...
    
    func addNodeWithParam(nodeName:String,timeDuration:String?,baseUrl:String?,keyId:String?,keySecret:String?,messageText:String?,voiceType:String?,questionID:String?) {
        switch nodeName {
        case VidusInput.VidusInputNode.simpleRecorderNode.rawValue:
            inputParameters[VidusInput.SDKInputParameter.nodeName.rawValue] = nodeName
            inputParameters[VidusInput.SDKInputParameter.timeDuration.rawValue] = timeDuration
            nodeNameArray.append(nodeName)
            inputParam.append(inputParameters)
            break
        case VidusInput.VidusInputNode.challengeCodeNode.rawValue:
            inputParameters[VidusInput.SDKInputParameter.nodeName.rawValue] = nodeName
            inputParameters[VidusInput.SDKInputParameter.challengeCodeText.rawValue] = messageText
            inputParameters[VidusInput.SDKInputParameter.baseUrl.rawValue] = baseUrl
            inputParameters[VidusInput.SDKInputParameter.keyId.rawValue] = keyId
            inputParameters[VidusInput.SDKInputParameter.keySecret.rawValue] = keySecret
            nodeNameArray.append(nodeName)
            inputParam.append(inputParameters)
            break
        case VidusInput.VidusInputNode.challengeTextNode.rawValue:
            inputParameters[VidusInput.SDKInputParameter.nodeName.rawValue] =  nodeName
            inputParameters[VidusInput.SDKInputParameter.challengeCodeText.rawValue] = messageText
            inputParameters[VidusInput.SDKInputParameter.timeDuration.rawValue] = timeDuration
            nodeNameArray.append(nodeName)
            inputParam.append(inputParameters)
            break
        case VidusInput.VidusInputNode.declarationNode.rawValue:
            inputParameters[VidusInput.SDKInputParameter.nodeName.rawValue] = nodeName
            inputParameters[VidusInput.SDKInputParameter.voiceType.rawValue] = voiceType
            inputParameters[VidusInput.SDKInputParameter.challengeCodeText.rawValue] = messageText
            nodeNameArray.append(nodeName)
            inputParam.append(inputParameters)
            break
        case VidusInput.VidusInputNode.oSVRecorderNode.rawValue:
            inputParameters[VidusInput.SDKInputParameter.nodeName.rawValue] = nodeName
            inputParameters[VidusInput.SDKInputParameter.timeDuration.rawValue] = timeDuration
            nodeNameArray.append(nodeName)
            inputParam.append(inputParameters)
            break
        case VidusInput.VidusInputNode.oSVChallengeTextNode.rawValue:
            inputParameters[VidusInput.SDKInputParameter.nodeName.rawValue] = nodeName
            inputParameters[VidusInput.SDKInputParameter.challengeCodeText.rawValue] = messageText
            inputParameters[VidusInput.SDKInputParameter.timeDuration.rawValue] = timeDuration
            nodeNameArray.append(nodeName)
            inputParam.append(inputParameters)
            break
         case VidusInput.VidusInputNode.PIVNode.rawValue:
            inputParameters[VidusInput.SDKInputParameter.nodeName.rawValue] = nodeName
            inputParameters[VidusInput.SDKInputParameter.challengeCodeText.rawValue] = messageText
            inputParameters[VidusInput.SDKInputParameter.baseUrl.rawValue] = baseUrl
            inputParameters[VidusInput.SDKInputParameter.keyId.rawValue] = keyId
            inputParameters[VidusInput.SDKInputParameter.keySecret.rawValue] = keySecret
            inputParameters[VidusInput.SDKInputParameter.questionID.rawValue] = questionID
            nodeNameArray.append(nodeName)
            inputParam.append(inputParameters)
            break
         case VidusInput.VidusInputNode.PIVCNode.rawValue:
            inputParameters[VidusInput.SDKInputParameter.nodeName.rawValue] = nodeName
            inputParameters[VidusInput.SDKInputParameter.fileTemplate.rawValue] = languageCode
            inputParameters[VidusInput.SDKInputParameter.titleTemplate.rawValue] = questionTemplatePath
            nodeNameArray.append(nodeName)
            inputParam.append(inputParameters)
        default:
            print("Error: Node is empty")
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

8. **[PIVC Node](#pivc-node)**



The Input Nodes are explained below,

#### Simple Recorder Node

Captures a video recording with a user defined time.

<div>
<table style="width:100%">
  <tr>
    <th bgcolor="#F1F1F1" colspan="2">Public Methods</th>
  </tr>
  <tr>
    <td><b>nodeParameters[VidusInput.SDKInputParameter.timeDuration.rawValue] = </b>videoRecordTime</td>
    <td>Sets the time(<em>String</em>) taken for the recording of the node.
    </br></br> Values should be between <b> 10 and 100 </b> </td>
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
    <td><b>nodeParameters[VidusInput.SDKInputParameter.challengeCodeText.rawValue] = </b>videoChallengeCodeText</td>
    <td>Sets the text that will be spoken prior to displaying the code.</td>
  </tr>
  <tr>
  <td><b>nodeParameters[VidusInput.SDKInputParameter.baseUrl.rawValue] = </b>videoApiBaseUrl</td>
 <td>Vidus API Base URL.
 </tr>
 <tr>
 <td><b>nodeParameters[VidusInput.SDKInputParameter.keyId.rawValue] = </b>Key iD</td>
 <td>Sets the key id.</td>
 <tr>
 <tr>
 <td><b>nodeParameters[VidusInput.SDKInputParameter.keySecret.rawValue] = </b>Key SECRET</td>
 <td>Sets the Key Secret id.</td>
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
 <td><b>nodeParameters[VidusInput.SDKInputParameter.challengeCodeText.rawValue] = </b>videoChallengeText</td>
 <td>Sets the text that will be spoken.</td>
 </tr>
 <tr>
 <td><b>nodeParameters[VidusInput.SDKInputParameter.timeDuration.rawValue]</b> videoChallengeTextTime</td>
 <td>Sets the recording time(<em></em>) for the node after the text is spoken.
 </br></br> Values should be between <b> 10 and 100 </b> </td>
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
 <td><b> nodeParameters[VidusInput.SDKInputParameter.challengeCodeText.rawValue] = </b>videoDeclarationText</td>
 <td>Sets the text that will be displayed.</td>
 </tr>
 <tr>
</td>
 </tr>
 <tr>
 <td><b>nodeParameters[VidusInput.SDKInputParameter.voiceType.rawValue] = </b>(<em>int videoDeclarationSpokenMethod</em>)</td>
 <td>(OPTIONAL) </br> Sets the way the text is to be spoken, 
 </br> Either by the user or by the machine.</br>
 </br> Values are </br> <b>VidusInput.VoiceType.byMacine.rawValue </br> VidusInput.VoiceType.byUser.rawValue </b>(Default Value) </td>
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
<td><b> nodeParameters[VidusInput.SDKInputParameter.timeDuration.rawValue] = </b>videoRecordTime</td>
<td>Sets the time(<em></em>) taken for the recording of the node.
</br></br> Values should be between <b> 10 and 100 </b> </td>
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
 <td><b>nodeParameters[VidusInput.SDKInputParameter.challengeCodeText.rawValue] = </b>videoChallengeText</td>
 <td>Sets the text that will be spoken.</td>
 </tr>
 <tr>
 <td><b>nodeParameters[VidusInput.SDKInputParameter.timeDuration.rawValue] = </b> videoChallengeTextTime</td>
 <td>Sets the recording time(<em></em>) for the node after the text is spoken.
 </br></br> Values should be between <b> 10 and 100 </b> </td>
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
    <td><b>nodeParameters[VidusInput.SDKInputParameter.challengeCodeText.rawValue] = </b>videoChallengeCodeText</td>
    <td>Sets the text that will be displayed.</td>
  </tr>
  <tr>
  <td><b>nodeParameters[VidusInput.SDKInputParameter.baseUrl.rawValue] = </b>videoApiBaseUrl</td>
 <td>Vidus API Base URL.
 </tr>
 <tr>
 <td><b>nodeParameters[VidusInput.SDKInputParameter.keyId.rawValue] = </b>Key iD</td>
 <td>Sets the key id.</td>
 <tr>
 <tr>
 <td><b>nodeParameters[VidusInput.SDKInputParameter.keySecret.rawValue] = </b>Key SECRET</td>
 <td>Sets the Key Secret id.</td>
 </tr>
</table>
</div>

#### PIVC Node

<div>
<table style="width:100%">
  <tr>
    <th bgcolor="#F1F1F1" colspan="2">Public Methods</th>
  </tr>
  <tr>
    <td><b>nodeParameters[VidusInput.SDKInputParameter.nodeName.rawValue] = </b>Nodename</td>
    <td>Sets the Node name.</td>
  </tr>
  <tr>
  <td><b>nodeParameters[VidusInput.SDKInputParameter.fileTemplate.rawValue] = </b>LanguageCode</td>
  <td>sets the language for text eg: For English langauge code is "en-IN" and for Hindi code is "hi-IN".
  </tr>
  <tr>
  <td><b>nodeParameters[VidusInput.SDKInputParameter.titleTemplate.rawValue] = </b>questionTemplatePath</td>
  <td>Sets the question text.</td>
  <tr>
  <tr>
 </table>
 </div>




## Vidus Error Codes

Following error codes will be returned on the `onVidusFailure` method of the callback

| CODE | DESCRIPTION                  |
| ---- | ---------------------------- |
| 803  | Camera Permission deny             |
| 804  | SDK is interrupted       |
| 805  | Vidus SDK license expire            |
| 806  | Vidus SDK license is invalid |
| 807  | Invalid Config         |
| 808  | Transaction fail       |
| 809  | Internet is not available             |
| 810  | Timeout             |
| 811  | Screen recording permision deny             |
| 812  | Upload audio to server fail           |
| 813  | Microphone permission deny            |
| 814  | Upload screen images fail            |
| 815  | Secret number verification fail            |
| 818  | Upload screen recorded video fail           |
| 819  | Timeout             |
| 820  | Minimum time duration for recording is not set            |
| 821  | Recording Video is Failed            |


## Help
For any queries/feedback , contact us at `support@frslabs.com` 

