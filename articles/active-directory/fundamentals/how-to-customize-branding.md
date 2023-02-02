---
title: Add company branding to your organization's sign-in page (preview) - Azure AD
description: Instructions about how to add your organization's branding to the sign-in experience.
services: active-directory
author: shlipsey3
manager: amycolannino

ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 01/31/2023
ms.author: sarahlipsey
ms.reviewer: almars
ms.custom: "it-pro, seodec18, fasttrack-edit"
ms.collection: M365-identity-device-management
---

# Configure your company branding (preview)

When users authenticate into your corporate intranet or web-based applications, Azure Active Directory (Azure AD) provides the identity and access management (IAM) service. You can add company branding that applies to all these sign-in experiences to create a consistent experience for your users.

The default sign-in experience is the global look and feel that applies across all sign-ins to your tenant. Before you customize any settings, the default Microsoft branding will appear in your sign-in pages. You can customize this default experience with a custom background image or color, favicon, layout, header, and footer. You can also upload a custom CSS.

The updated experience for adding company branding covered in this article is available as an Azure AD preview feature. To opt in and explore the new experience, go to **Azure AD** > **Preview features** and enable the **Enhanced Company Branding** feature. For more information about previews, see [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Instructions for the legacy company branding customization process can be found in the [Customize branding](customize-branding.md) article.

## User experience

You can customize the sign-in pages when users access your organization's tenant-specific apps. For Microsoft and SaaS applications (multi-tenant apps) such as <https://myapps.microsoft.com>, or <https://outlook.com> the customized sign-in page appears only after the user types their **Email**, or **Phone**, and select **Next**. 

Some of the Microsoft applications support the home realm discovery `whr` query string parameter, or a domain variable. With the home realm discovery and domain parameter, the customized sign-in page will appear immediately in the first step. 

In the following examples replace the contoso.com with your own tenant name, or verified domain name:

- For Microsoft Outlook `https://outlook.com/contoso.com` 
- For SharePoint online `https://contoso.sharepoint.com`
- For my app portal `https://myapps.microsoft.com/?whr=contoso.com` 
- Self-service password reset `https://passwordreset.microsoftonline.com/?whr=contoso.com`

## License requirements

Adding custom branding requires one of the following licenses:

- Azure AD Premium 1
- Azure AD Premium 2
- Office 365 (for Office apps)

For more information about licensing and editions, see the [Sign up for Azure AD Premium](active-directory-get-started-premium.md) article.

Azure AD Premium editions are available for customers in China using the worldwide instance of Azure AD. Azure AD Premium editions aren't currently supported in the Azure service operated by 21Vianet in China

## Before you begin

**All branding elements are optional. Default settings will remain, if left unchanged.** For example, if you specify a banner logo but no background image, the sign-in page shows your logo with a default background image from the destination site such as Microsoft 365. Additionally, sign-in page branding doesn't carry over to personal Microsoft accounts. If your users or guests authenticate using a personal Microsoft account, the sign-in page won't reflect the branding of your organization.

**Images have different image and file size requirements.** Take note of the image requirements for each option. You may need to use a photo editor to create the right size images. The preferred image type for all images is PNG, but JPG is accepted. 

1. Sign in to the [Azure portal](https://portal.azure.com/) using a Global Administrator account for the directory.

2. Go to **Azure Active Directory** > **Company branding** > **Customize**.
    - If you currently have a customized sign-in experience, you'll see an **Edit** button.

    ![Custom branding landing page with 'Company branding' highlighted in the side menu and 'Configure' button highlighted in the center of the page](media/how-to-customize-branding/customize-branding-getting-started.png)

The sign-in experience process is grouped into sections. At the end of each section, select the **Review + create** button to review what you have selected and submit your changes or the **Next** button to move to the next section.

!['Review + create' and 'Next: Layout' buttons from the bottom of the configure custom branding page](media/how-to-customize-branding/customize-branding-buttons.png)

## Basics

- **Favicon**: Select a PNG or JPG of your logo that appears in the web browser tab.

- **Background image**: Select a PNG or JPG to display as the main image on your sign-in page. This image will scale and crop according to the window size, but may be partially blocked by the sign-in prompt.

- **Page background color**: If the background image isn't able to load because of a slower connection, your selected background color appears instead.

## Layout

- **Visual Templates**: Customize the layout of your sign-in page using templates or custom CSS.

    - Choose one of two **Templates**: Full-screen or partial-screen background. The full-screen background could obscure your background image, so choose the partial-screen background if your background image is important.
    - The details of the **Header** and **Footer** options are set on the next two sections of the process.

- **Custom CSS**: Upload custom CSS to replace the Microsoft default style of the page. [Download the CSS template](https://download.microsoft.com/download/7/2/7/727f287a-125d-4368-a673-a785907ac5ab/custom-styles-template-013023.css).

## Header

If you haven't enabled the header, go to the **Layout** section and select **Show header**. Once enabled, select a PNG or JPG to display in the header of the sign-in page.

## Footer

If you haven't enabled the footer, go to the **Layout** section and select **Show footer**. Once enabled, adjust the following settings.

- **Show 'Privacy & Cookies'**: This option is selected by default and displays the [Microsoft 'Privacy & Cookies'](https://privacy.microsoft.com/privacystatement) link.
    
    Uncheck this option to hide the default Microsoft link. Optionally provide your own **Display text** and **URL**. The text and links don't have to be related to privacy and cookies.

- **Show 'Terms of Use'**: This option is also elected by default and displays the [Microsoft 'Terms of Use'](https://www.microsoft.com/servicesagreement/) link.

    Uncheck this option to hide the default Microsoft link. Optionally provide your own **Display text** and **URL**. The text and links don't have to be related to your terms of use.

    >[!IMPORTANT]
    >The default Microsoft 'Terms of Use' link is not the same as the Conditional Access Terms of Use. Seeing the terms here doesn't mean you've accepted those terms and conditions. 

    ![Customize branding on the Footer section](media/how-to-customize-branding/customize-branding-footer.png)

## Sign-in form

- **Banner logo**: Select a PNG or JPG image file of a banner-sized logo (short and wide) to appear on the sign-in pages.

- **Square logo (light theme)**: Select a square PNG or JPG image file of your logo to be used in browsers that are using a light color theme. This logo is used to represent your organization on the Azure AD web interface and in Windows.

- **Square logo (dark theme)** Select a square PNG or JPG image file of your logo to be used in browsers that are using a dark color theme. This logo is used to represent your organization on the Azure AD web interface and in Windows. If your logo looks good on light and dark backgrounds, there's no need to add a dark theme logo.

- **Username hint text**: Enter hint text for the username input field on the sign-in page. If guests use the same sign-in page, we don't recommend using hint text here.

- **Sign-in page text**: Enter text that appears on the bottom of the sign-in page. You can use this text to communicate additional information, such as the phone number to your help desk or a legal statement. This page is public, so don't provide sensitive information here. This text must be Unicode and can't exceed 1024 characters.

    To begin a new paragraph, use the enter key twice. You can also change text formatting to include bold, italics, an underline, or clickable link. Use the following syntax to add formatting to text: 

    > Hyperlink: `[text](link)` 
        
    > Bold: `**text**` or `__text__` 
          
    > Italics: `*text*` or `_text_` 
          
    > Underline: `++text++` 
         
    > [!IMPORTANT]
    > Hyperlinks that are added to the sign-in page text render as text in native environments, such as desktop and mobile applications.

- **Self-service password reset**:
    - Show self-service password reset (SSPR): Select the checkbox to turn on SSPR. 
    - Common URL: Enter the destination URL for where your users will reset their passwords. This URL appears on the username and password collection screens.
    - Username collection display text: Replace the default text with your own custom username collection text.
    - Password collection display text: Replace the default text with your own customer password collection text.

## Review

All of the available options appear in one list so you can review everything you've customized or left at the default setting. When you're done, select the **Create** button. 

Once your default sign-in experience is created, select the **Edit** button to make any changes. You can't delete a default sign-in experience after it's created, but you can remove all custom settings.

## Customize the sign-in experience by browser language

To create an inclusive experience for all of your users, you can customize the sign-in experience based on browser language.

1. Sign in to the [Azure portal](https://portal.azure.com/) using a Global Administrator account for the directory.

2. Go to **Azure Active Directory** > **Company branding** > **Add browser language**.

The process for customizing the experience is the same as the [default sign-in experience](#basics) process, except you must select a language from the dropdown list in the **Basics** section. We recommend adding custom text in the same areas as your default sign-in experience. 

## Next steps

- [Learn more about default user permissions in Azure AD](../fundamentals/users-default-permissions.md)

- [Manage the 'stay signed in' prompt](active-directory-users-profile-azure-portal.md#learn-about-the-stay-signed-in-prompt)
