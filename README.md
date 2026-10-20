# FormBuilderSDK

[![Platforms](https://img.shields.io/badge/Platforms-iOS-yellowgreen?style=flat)](https://img.shields.io/badge/Platforms-iOS-Green?style=flat)
[![Swift](https://img.shields.io/badge/Swift-5.10-orange?style=flat)](https://img.shields.io/badge/Swift-5.10-Orange?style=flat)
[![Swift Package Manager](https://img.shields.io/badge/Swift_Package_Manager-compatible-orange?style=flat)](https://img.shields.io/badge/Swift_Package_Manager-compatible-orange?style=flat)

## Requirements
| Platform | Minimum Swift Version | Installation |
| --- | --- | --- |
| iOS 15.0+ | 5.10 | [Swift Package Manager](#swift-package-manager) |

## Installation

### Swift Package Manager

The [Swift Package Manager](https://swift.org/package-manager/) is a tool for automating the distribution of Swift code and is integrated into the `swift` compiler. 

Once you have your Swift project set up, adding FormBuilderSDK as a dependency is as easy as adding it to the `dependencies` value of your `Package.swift`.

```swift
dependencies: [
	.package(url: "https://github.com/Rabbitregtech/FormBuilder.git", .upToNextMajor(from: "1.5.0"))
]
```

## Documentation
You can Option+Click on the methods and properties to view documentation.

## How to use?

The basic usage:

```swift
let formBuilder = FormBuilder(instanceURL: {YOUR_BACKEND_URL})

formBuilder.startProcess(withId: {A_PROCESS_ID}, callingServiceToken: {TOKEN}, delegate: delegate, didStartProcess: { result in
	switch result {
	case .success:
		// do something
	case .failure(let error):
		// handle error
	}
})
```

This creates an instance of `FormBuilder` pointing to your backend and then opens a new interface based on the type of the process corresponding to the provided ID. The `callingServiceToken` argument is a unique client identifier that will be stored in the submission.

In the `startProcess(withId:callingServiceToken:delegate:didStartProcess:)` method you should also pass an object conforming to the `FBSDelegate` protocol. The `FBSDelegate` protocol defines three methods. The first one is `closedForm(onStepWithID:)` that is called when the user closes the FormBuilder interface and will give you the ID of the step where the user closed the framework UI. The second one is `processResult(success:)` that is called when KYC status for fail or success is received. The third one is `postToHostUI(status:)` that is called when a field with specific type is received.
The `didStartProcess` closure tells you whether opening the process was successful or there was some error.

---

```swift

let formBuilder = FormBuilder(instanceURL: {YOUR_BACKEND_URL})

formBuilder.openStep(withId: {A_STEP_ID}, delegate: delegate, didOpenStep: { result in
	switch result {
	case .success:
		// do something
	case .failure(let error):
		// handle error
	}
})
```

This creates an instance of `FormBuilder` pointing to your backend and then opens a new interface of a previously created process at the step which the user has reached before (e.g. if they have interrupted it for some reason). It can also just be the first step for a process that the backend has preconfigured in some way.

In the `openStep(withId:delegate:didOpenStep:)` method you should also pass an object conforming to the `FBSDelegate` protocol. The `FBSDelegate` protocol defines three methods. The first one is `closedForm(onStepWithID:)` that is called when the user closes the FormBuilder interface and will give you the ID of the step where the user closed the framework UI. The second one is `processResult(success:)` that is called when KYC status for fail or success is received. The third one is `postToHostUI(status:)` that is called when a field with specific type is received.
The `didOpenStep` closure tells you whether opening the process was successful or there was some error.

---

You provide an `URL` in the `FormBuilder` initialiser which points to your own FormBuilder/KYC instance.

```swift
let KYCBackendURL = URL(string: "https://your-kyc-backend.net/ss")!

let formBuilder = FormBuilder(instanceURL: KYCBackendURL) 
``` 

Optionally you can pass an `FBSAppearance` object to the `FormBuilder` initialiser with which you can configure some of the appearance of the labels, fields, buttons:

```swift
let appearance = FBSAppearance()
appearance.backgroundColor = .systemIndigo
appearance.tintColor = .systemGreen
appearance.buttonCornerStyle = .capsule
appearance.textColor = .white

let formBuilder = FormBuilder(instanceURL: KYCBackendURL, appearance: appearance)
```

If you don’t pass an appearance object, the SDK will use default values.

---

## Universal Links (for IBAN Verification)

If your verification flow includes the **IBAN Verification / Open Banking step**, users will be redirected to their banking app or Safari to authorize their account. To seamlessly return the user back into your app once the bank authorization completes, configure **Universal Links**.

> **Note:** Contact the FormBuilder team to receive your assigned `{MERCHANT_SLUG}`.

### 1. Enable Associated Domains Capability
1. Open your project in Xcode.
2. Select your main app target and go to the **Signing & Capabilities** tab.
3. Click **+ Capability** and select **Associated Domains**.
4. In the **Domains** list, add both environments:
   - `applinks:dev3.kyc.rbbt.dev` *(Development)*
   - `applinks:id.rbbt.dev` *(Production)*

### 2. Apple App Site Association (AASA) Registration
Apple requires domain verification via the `apple-app-site-association` file hosted on our servers.
Please provide the FormBuilder support team with:
- Your Apple **Team ID** (e.g. `ABC123XYZ0`)
- Your **Bundle Identifier** (e.g. `com.yourcompany.yourapp`)

This registers your app ID (`<TeamID>.<BundleID>`) in `https://id.rbbt.dev/.well-known/apple-app-site-association` for the path `/iris-redirect/{MERCHANT_SLUG}/*`.

### 3. Handling Incoming Links (Optional)
When returning via Universal Link, the SDK automatically polls the verification status and advances the flow as soon as the app becomes active. If your app implements custom scene handling in `SceneDelegate`, ensure Universal Links are permitted to pass through:

```swift
func scene(_ scene: UIScene, continue userActivity: NSUserActivity) {
    guard userActivity.activityType == NSUserActivityTypeBrowsingWeb,
          let url = userActivity.webpageURL else { return }
    // Universal link returned user to app — SDK polling automatically resumes
}
```

