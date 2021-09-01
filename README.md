# Biometric Authentication Fingerprint Example

See the blog post of this project [here][0]

-------------

## Project Presentation

<br>

1. First, let's add the Biometric library to our project.
    ```kotlin
    dependencies {
      implementation("androidx.biometric:biometric:1.2.0-alpha03")
    }
    ```
    <br>
2. After adding our library, let's do the necessary coding for situations such as whether the device has biometric authentication or not.
    ```kotlin
    val biometricManager = BiometricManager.from(this)
    when (biometricManager.canAuthenticate(BIOMETRIC_STRONG or DEVICE_CREDENTIAL)) {
    BiometricManager.BIOMETRIC_SUCCESS ->
        Toast.makeText(this,"App can authenticate using biometrics.",Toast.LENGTH_SHORT).show()
    BiometricManager.BIOMETRIC_ERROR_NO_HARDWARE ->
        Toast.makeText(this,"No biometric features available on this device.",Toast.LENGTH_SHORT).show()
    BiometricManager.BIOMETRIC_ERROR_HW_UNAVAILABLE ->
        Toast.makeText(this,"Biometric features are currently unavailable.",Toast.LENGTH_SHORT).show()
    BiometricManager.BIOMETRIC_ERROR_NONE_ENROLLED ->
        Toast.makeText(this,"The device does not have any biometric credentials",Toast.LENGTH_SHORT).show()
    BiometricManager.BIOMETRIC_ERROR_SECURITY_UPDATE_REQUIRED ->
        Toast.makeText(this,"A security vulnerability has been discovered and the sensor is unavailable until a security update has addressed this issue.",Toast.LENGTH_SHORT).show()
    BiometricManager.BIOMETRIC_ERROR_UNSUPPORTED ->
        Toast.makeText(this,"A given authenticator combination is not supported by the device.",Toast.LENGTH_SHORT).show()
    BiometricManager.BIOMETRIC_STATUS_UNKNOWN ->
        Toast.makeText(this,"Unable to determine whether the user can authenticate.",Toast.LENGTH_SHORT).show()
    }
    ```
<br>

3. Now let's show the user the authentication dialog. First of all, let's create the following variables in the class.
    ```kotlin
    private lateinit var executor: Executor
    private lateinit var biometricPrompt: BiometricPrompt
    private lateinit var promptInfo: BiometricPrompt.PromptInfo
    ```
    <br>
    
4. Then let's define these values in order.
    ```kotlin
    executor = ContextCompat.getMainExecutor(this)
    biometricPrompt = BiometricPrompt(this, executor,
    object : BiometricPrompt.AuthenticationCallback() {
        override fun onAuthenticationError(errorCode: Int, errString: CharSequence) {
            super.onAuthenticationError(errorCode, errString)
            Toast.makteText(this,errString.toString(),Toast.LENGTH_SHORT).show()
        }

        override fun onAuthenticationSucceeded(result: BiometricPrompt.AuthenticationResult) {
            super.onAuthenticationSucceeded(result)
            Toast.makteText(this,"Authentication Success",Toast.LENGTH_SHORT).show()
            startActivity(Intent(this@MainActivity,SecondActivity::class.java))
        }

        override fun onAuthenticationFailed() {
            super.onAuthenticationFailed()
            Toast.makteText(this,"Authentication Failed",Toast.LENGTH_SHORT).show()
        }
    })
    ```
    <br>

5. Now, let's define the promptInfo variable.
    ```ruby
    promptInfo = BiometricPrompt.PromptInfo.Builder()
    .setTitle("Biometric login for my app")
    .setSubtitle("Log in using your biometric credential")
    .setNegativeButtonText("Cancel")
    .build()
    ```
    <br>
    
5. Finally, we use the authenticate function to show our dialog when the button is clicked and give the biometricPrompt the promptInfo.
    ```ruby
    binding.button.setOnClickListener {
        biometricPrompt.authenticate(promptInfo)
    }
    ```
    <br>

-----------
## And Result

<p align="center">
  <img src="https://static.wixstatic.com/media/178f5d_7c0b5818b98f43aaaa747eefc29cec49~mv2.gif" alt="GIF" />
</p>

[0]: https://www.mobiler.dev/post/android-de-biyometrik-kimlik-dogrulama
