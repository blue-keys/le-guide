# How to install Windows 10 root certificates \[EASY STEPS\]

 Last update:March 27, 2019

![what to do if Windows 10 can&apos;t find Wi-Fi network](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%20886%20590'%3E%3C/svg%3E)

**To fix various PC problems, we recommend Restoro PC Repair Tool:** This software will repair common computer errors, protect you from file loss, malware, hardware failure and optimize your PC for maximum performance. Fix PC issues and remove viruses now in 3 easy steps:

1.  [**Download Restoro PC Repair Tool**](https://www.restoro.com/includes/route.php?tracking=Gures&exec=run&banner=WR_top_EN&adgroup=https://windowsreport.com/install-windows-10-root-certificates/) that comes with Patented Technologies \(patent available [here](https://patents.google.com/patent/US20100064285)\).
2.  Click **Start Scan** to find Windows issues that could be causing PC problems.
3.  Click **Repair All** to fix issues affecting your computer's security and performance

* _Restoro has been downloaded by 0 readers this month._

Root certificates are public key certificates that help your [browser](https://windowsreport.com/best-browser-multiple-tabs/) determine whether communication with a website is genuine and is based upon whether the issuing authority is trusted and if the digital certificate remains valid.

If a digital certificate is not from a trusted authority, you’ll get an error message along the lines of “_There is a problem with this website’s security certificate_” and the browser might block communication with the website.

Windows 10 has built-in certificates and automatically updates them. However, you can still manually add more root certificates to Windows 10 from certificate authorities \(CAs\).

There are numerous certificate issuing authorities, with Comodo and Symantec among the best known.

## How can I add Windows 10 root certificates manually?

1. [Install certificates from trusted CAs](https://windowsreport.com/install-windows-10-root-certificates/#1)
2. [Install Certificates with the Microsoft Management Console](https://windowsreport.com/install-windows-10-root-certificates/#2)

### Method 1: Install certificates from trusted CAs <a id="1"></a>

This is how you can add digital certificates to Windows 10 from trusted CAs.

1. First, you’ll need to download a root certificate from a CA. For example, you could download one from the [GeoTrust site](https://www.geotrust.com/resources/root-certificates/).
2. Next, open Local Security Policy in Windows by pressing the Win key + R [hotkey](https://windowsreport.com/windows-10-keyboard-shortcuts/) and entering ‘secpol.msc’ in Run’s text box. Note that Windows 10 Home edition doesn’t include the Local Security Policy editor. If your Windows key doesn’t work, check this [quick guide](https://windowsreport.com/windows-key-not-working-windows-10/) to fix it.
3. Then, click **Public Key Policies** and **Certificate Path Validation Settings** to open a Certificate Path Validation Settings Properties window.
4. Click the Stores tab and select the **Define these policy settings** check box.
5. Select the **Allow user trusted root CAs to be used to validate certificates** and **Allow users to trust peer trust certificates** options if they’re not already selected.
6. You should also select the **Third-Party Root CAs and Enterprise Root CAs** checkbox and press the **Apply** &gt; **OK** buttons to confirm the selected settings.
7. Next, press the Win key + R hotkey and enter ‘certmgr.msc’ in Run’s text box to open the window shown in the snapshot directly below. That’s the Certification Manager which lists your digital certificates.[![certificates manager](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%20626%20431'%3E%3C/svg%3E)](https://cdn.windowsreport.com/wp-content/uploads/2017/03/digital-certificate.jpg)
8. Click **Trusted Root Certification Authorities** and right-click **Certificates** to open a context menu.
9. Select **All Tasks** &gt; **Import** on the context menu to open the window shown below.[![certificate import wizard](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%20535%20523'%3E%3C/svg%3E)](https://cdn.windowsreport.com/wp-content/uploads/2017/03/digital-certificate2.jpg)
10. Press the **Next** button, click **Browse,** and then select the digital certificate root file saved to your HDD.
11. Press **Next** again to select the **Automatically select the certificate store based on the type of certificate** option.
12. Then you can press **Next** &gt; **Finish** to wrap up the import wizard. A window will open confirming that “_the import was successful._”

Most Windows 10 users have no idea how to edit the Group Policy. Learn how you can do it by reading this [simple article](https://windowsreport.com/edit-group-policy-windows-8-1/).

[**You don’t have the Group Policy Editor on your Windows PC? Get it right now in just a couple of easy steps.**](https://windowsreport.com/install-group-policy-editor-windows-10-home/)

### Method 2: Install Certificates with the Microsoft Management Console <a id="2"></a>

1. You can also add digital certificates to Windows with the Microsoft Management Console. Press the Win key + R hotkey and input ‘mmc’ in Run to open the window below.[![microsoft management console](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%201011%20514'%3E%3C/svg%3E)](https://cdn.windowsreport.com/wp-content/uploads/2017/03/digital-certificate3.jpg)
2. Click **File** and then select **Add/Remove Snap-ins** to open the window in the snapshot below.[![console root mmc](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%20674%20477'%3E%3C/svg%3E)](https://cdn.windowsreport.com/wp-content/uploads/2017/03/digital-certificate4.jpg)
3. Next, you should select **Certificates** and press the **Add button**.
4. A Certificates Snap-in window opens from which you can select **Computer account** &gt; **Local Account**, and press the **Finish** button to close the window.
5. Then press the **OK** button in the Add or Remove Snap-in window.
6. Now you can select **Certificates** and right-click **Trusted Root Certification Authorities** on the MMC console window as below.[![import root certificate](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%20793%20371'%3E%3C/svg%3E)](https://cdn.windowsreport.com/wp-content/uploads/2017/03/digital-certificate5.jpg)
7. Then you can click **All Tasks** &gt; **Import** to open the Certificate Import Wizard window from which you can add the digital certificate to Windows.

#### Run a System Scan to discover potential errors

![Restoro Scan](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%200%200'%3E%3C/svg%3E)

Click **Start Scan** to find Windows issues.

![Restoro Fix](data:image/svg+xml,%3Csvg%20xmlns='http://www.w3.org/2000/svg'%20viewBox='0%200%200%200'%3E%3C/svg%3E)

Click **Repair All** to fix issues with Patented Technologies.

 Run a PC Scan with Restoro Repair Tool to find errors causing security problems and slowdowns. After the scan is complete, the repair process will replace damaged files with fresh Windows files and components.

If Microsoft Management Console can’t create a new document, follow the easy steps in [this guide](https://windowsreport.com/microsoft-management-console-new-document/) to solve the issue.

[**Can’t load the Microsoft Management Console? This step-by-step guide will help you sort things out.**](https://windowsreport.com/management-console-not-loading/)

Now you’ve installed a new trusted root certificate in Windows 10. You can add many more digital certificates to that OS and other Windows platforms in a similar manner.

Just make sure that the third-party digital certificates come from trusted CAs, such as GoDaddy, DigiCert, Comodo, GlobalSign, Entrust and Symantec.

If you have any more suggestions or questions, leave them in the comments section below and we’ll certainly check them out.

**Was this page helpful?**

**Thanks for letting us know! You can also help us by leaving a review on** [**MyWOT**](https://www.mywot.com/en/scorecard/windowsreport.com) **or** [**Trustpilot**](https://www.trustpilot.com/review/windowsreport.com)**.**

**Get the most from your tech with our daily tips**

\*\*\*\*

**Source :** 

{% embed url="https://windowsreport.com/install-windows-10-root-certificates" %}

\*\*\*\*

