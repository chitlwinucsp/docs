Window Autopilot
----------------

window autopilot ဆိုတာ အချိန်ကုန်သက်သက်သာသာနဲ့ setup လုပ်နိုင်ပါတယ်။ 

window autopilot က window home or creator verion ဆိုတာမျိုးတွေမှာတော့ သုံးလို့ ရမှာ မဟုတ်ဘူး။

window autopilot ကိုသုံးဖို့ရာ အဓိက ကျတာကတော့ Adzure AD ပါ။ 
vendor ကနေဝယ်လာတဲ့ device spec ကို csv ဖိုင်သို့မဟုတ် support  ပေးထားတဲ့ ms store business or intune ကို upload လုပ်တာနဲ့ 
az ad နဲ့ ချိတ်ပြီး သူ့ဘာသာ အလုပ်လုပ်သွားမှာ ဖြစ်တယ်။

အောက်က ဆိုဒ်တွေကိုတော့ browse လုပ်နိုင်ရပါမယ်။ အဲ့ဒါအပြင် http, https, udp/ntp တွေလဲ allow ဖြစ်ရပါမယ်။


* go.microsoft.com
* login.microsoftonline.com
* login.live.com
* account.live.com
* signup.live.com
* licensing.mp.microsoft.com
* licensing.md.mp.microsoft.com
* ctldl.windowsupdate.com
* download.windowsupdate.com

window autopilot ဆိုတာ cloud service ဖြစ်တဲ့အတွက်ကြောင့်မို့လိုလဲ internet connection ရပြီး ကောင်းရပါမယ်။


နောက်တစ်ဆင့်အနေ့နဲ့ device ids ကို ပြင်ဆင်ကြမယ်။ 
1. hareware ids of device
2. upload the hareware ids
3. Creating a window autopilot profile
4. apply window autopilot deployment profile


### 1, 2 Creating and uploading the hardware ids ###
------
Original Equipment Manufacturer (OEM) ဆီကနေ ရတဲ့ အောက်ဖော်ပြပါ ၃ ခုကို csv  ဖိုင်မှာ သိမ်းရပါမယ်။
1. device serial number
2. window product id
3. hareware hash

OEM က မပေးဘူးဆိုရင် ကိုယ့်ဘာသာ csv ဆောက်ဖို့အတွက် powershell ကို သုံးရပါမယ်။
အရင်ဆုံး အောက်ပါ script ကို run ပါ။

```
Install-Script -Name Get-WindowsAutopilotInfo
```

run ပြီးသွားတဲ့အခါမှာ csv generate ထုတ်ဖို့ အောက်ကဟာကို သုံးပါ။

```
Get-WindowsAutopilotInfo.ps1 –OutputFile D:\\Devices\\Device1.csv
```

csv ထုတ်ပြီးသွားရင်တော့ upload တင်ရပါတော့မယ်။ ကိုယ်က ms store for business or intune မှာ admin role ဖြစ်ရပါမယ်။

ms intune ကို သုံးရင်တော့ အောက်ကအတိုင်းသုံးရပါမယ်။
csv ထုတ်ဖို့ဆိုရင် 
```
Get-WindowsAutoPilotInfo.ps1 -online -GroupTag "Autopilot-Devices" -Assign
```

1. In Microsoft Intune admin center, navigate to **Devices > Enroll Devices > Devices**. Select **Import**.
2. Browse and locate your CSV file.
3. Import the file.
4. After import is complete, select Device enrollment, select Windows enrollment, select Windows Autopilot, select Devices and then select **Sync**.
5. Refresh the view to see the new devices.

   ![Reference](https://learn.microsoft.com/en-us/training/wwl/deploy-devices-windows-autopilot/media/windows-autopilot-service-white-4f335827.png)

### 3. Creating a window autopilot profile ###
------
နောက်တစ်ဆင့်အနေနဲ့ လုပ်ကိုလုပ်ရမှာ အဆင့်ကတော့ Windows Autopilot deployment profile ပါ။ အဲ့ဒါကို လုပ်ဖို့ဆိုရင်တော့ အောက်ပါ အဆင့်အတိုင်း ms intune ကနေ သုံးပြီး လုပ်ရပါမယ်။
1.Sign in to Intune as a global admin, and in the portal, select Device enrollment, select Windows enrollment, select Deployment Profiles and then select Create Profile. Provide a Name and Description.
2. Then, for Deployment mode, choose between user-driven and self-deploying. With the former, the user specifies credentials during deployment, and devices with this profile are associated with the user account. With the latter, user credentials aren't required to enroll the device, and devices with this profile aren't associated with the user account.
3. Next, configure the Out-of-box experience (OOBE) settings, which include: language, keyboard, EULA, privacy settings, and user account.
4. Finally, create the profile. The Autopilot deployment profile is now available to assign to devices.

profile လုပ်ပြီးတော့ ကိုယ် assign ချချင်တဲ့ ဟာကိုရွေးပြီး apply လုပ်ရပါမယ်။

 oobe normal နဲ့ autopilot ကိုနှိုင်းယှဥ်ရမယ်ဆိုရင်တော့ 
oobe ကဆိုရင် vendor ဆီကဝယ်ပြီး စက်ကို ဖွင့်လိုက်တာနဲ့
*What is your preferred keyboard layout?
*Do you accept the Microsoft Software License Terms?
*Should the computer join AD DS or Azure AD?
* Which privacy settings should you use?

စတာတွေ တစ်ခုချင်းစီ စိစစ်ပြီး ထည့်ရတဲ့အတွက်ကြောင့် IT နယ်ပယ်က မဟုတ်တဲ့ သူတွေဟာဆိုရင် ခက်ခဲ့တာ အခြေအနေတစ်ခုပါ။
![ref link](https://learn.microsoft.com/en-us/training/wwl/deploy-devices-windows-autopilot/media/microsoft-account-sign-d6997be5.png)

autopilot နဲ့သာဆို
![auto pilot splash screen](https://learn.microsoft.com/en-us/training/wwl/deploy-devices-windows-autopilot/media/azure-active-directory-sign-9f107960.png)

Windows Autopilot supports several deployment scenarios depending on the desired experience:

* Windows Autopilot user-driven mode
* Windows Autopilot self-deploying mode
* Autopilot for existing devices
* Windows Autopilot for pre-provisioned deployment
* Windows Autopilot reset
 
