# F5 DoD Warning Banner Configuration Guide

This document outlines the steps required to add the standard DoD Warning Banner configuration to an F5 virtual server, where intermediary access controls for web applications are provided by deployed F5 Access Policy Manager.

## Background

The use of a DoD Warning Banner is a Defense Information Security Agency (DISA) Security Technical Implementation Guide (STIG) requirement that is typically assigned as a Category III line item.

**STIG Requirement:**
> "The BIG-IP Core implementation must be configured to display the Standard Mandatory DoD-approved Notice and Consent Banner before granting access to virtual servers. (STIG ID: F5BI-LT-000023)"

## Prerequisites

- Familiarity with F5's GUI and basic administration of the F5 application delivery controller
- **Important:** Do not perform these procedures on an access policy that is applied to an application virtual server that is actively processing client logins
- This process should be performed on an offline test instance of an access policy and migrated to a production virtual server upon successful testing and acceptance of the modified access policy operation

## Configuration Steps

### Step 1: Select Application APM Access Policy

1. Navigate to **Main → Access Policy → Access Profiles → Access Profiles List**
2. Select the access profile you want to modify (e.g., "Test-Application_CAC_Auth")
3. Click **Edit** on the relevant line to launch the APM visual policy editor

<img width="539" height="124" alt="Image" src="https://github.com/user-attachments/assets/952397f4-c181-42a1-9b27-bf06b0c26de2" />

### Step 2: Understand APM Access Policy Structure

- DoD Warning Banners should be created at the start of access policy evaluation for all applications that face DoD networks
- This ensures that a user must accept the warning prior to user evaluation by APM, which includes the retrieval of the user's PKI certificate or login credentials
- The start of the APM access policy is the first connection point into the access policy, represented by the plus "+" symbol immediately following the policy "Start" fallback path
- Access policies are displayed visually and should be read from left to right, with the policy endpoint termination flags at the far right

### Step 3: Add Message Box Item to Access Policy

1. Click on the **"+"** sign following the **"Start"** flag to bring up APM's component menu
2. Select the **"General Purpose"** tab
3. Click the radio button for **"Message Box"**
4. Click **"Add Item"**
5. In the Message Box configuration panel:
   - Change the **Name** field in the Properties tab to **"DoD Banner"**
   - Add the text **"Look in advanced customization!"** in the Message field
   - Click **"Save"** at the bottom of the message box configuration panel
6. Click **"Apply Access Policy"** in yellow text
7. Click **"Close"** in the green box in the top right
8. If your browser notifies you that the webpage is trying to close the tab, click **"OK"**

### Step 4: Customize the DoD Warning Banner Code

1. In the F5 Admin GUI, navigate to **Main → Access Policy → Customization → Advanced**
2. Expand the **"Customization Settings"** tree for the access policy being modified (e.g., `/Common/Test-Application_CAC_Auth`)
3. Expand the subsections: **Access Policy → Message Panes → DoD Warning Banner**
4. Select the **"message_box.inc"** file to display its HTML code in the Advanced Customization Editor
5. Copy one of the DoD banner HTML codes provided (Green, Red, or Yellow banner)
6. In the F5 Advanced Customization editor:
   - Select all existing text and delete it
   - Paste the copied HTML code starting at line 1
   - Validate that the code is exactly 248 lines with Line 248 being: `</html>`
   - If incorrect, use the **"Revert"** button and try again
7. Click **"Save Draft"** on the top right of the editor panel
8. Click **"Yes"** when prompted to save the file

**Important:** Do not edit any portion of the provided code without testing, or unexpected behavior in APM may occur.

### Step 5: Apply the Access Policy, Synchronize the Configuration, and Test the Banner

1. Click **"Save"** in the Advanced Editor window
2. Click **"Apply Access Policy"**
3. Synchronize the cluster configuration
4. Test the modified policy using a test virtual server
5. Ensure the operation of the **"I ACKNOWLEDGE AND CONSENT"** button allows the user to proceed through access policy evaluation successfully

## Available Banner Types

The guide includes three color variants of the DoD Warning Banner:

### Green Banner
- Border color: Green (#00b000)
- Button hover: Green gradient

### Red Banner  
- Border color: Red (#e60000)
- Button hover: Red gradient

### Yellow Banner
- Border color: Yellow (#e6c200) 
- Button hover: Yellow gradient

## Banner Features

All banner implementations include:
- Responsive design for various screen sizes
- Accessibility features (focus indicators, semantic markup)
- Modern CSS animations and transitions
- Loading state prevention for double-clicks
- Session management JavaScript integration
- DoD-compliant consent language and formatting

## Testing and Validation

After implementation:
1. Verify the banner displays correctly on the test virtual server
2. Confirm the "I ACKNOWLEDGE AND CONSENT" button functions properly
3. Test that users can proceed through the access policy after consent
4. Validate the banner test matches DoD requirements
5. Only migrate to production after successful testing and acceptance
