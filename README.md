<p align="center"><img src="images/logo.png" width="50%" /></p>
<h1 align="center">TestMaverick SDk</h1>

## Revision History

<table>
    <thead>
        <tr>
            <th>Date</th>
            <th>Version No</th>
            <th>Author</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>22<sup>nd</sup> September, 2023</td>
            <td>1.0</td>
            <td>Zeus</td>
        </tr>
        <tr>
            <td>24<sup>th</sup> September, 2023</td>
            <td>1.1</td>
            <td>Zeus</td>
        </tr>
        <tr>
            <td>20<sup>th</sup> October, 2023</td>
            <td>1.2</td>
            <td>Zeus</td>
        </tr>
</table>

## Table of Content

<ol>
    <li> <a href="#introduction">Introduction</a> </li>
    <li> <a href="#overview">Overview</a> </li>
    <li> <a href="#authentication-and-security">Authentication and Security</a> </li>
    <li> 
        <a href="#revision-history">Auto Proctoring SDK</a> 
        <ol type="a">
            <li> <a href="#revision-history">Overview</a></li>
            <li> <a href="#revision-history">Sequence Diagram</a></li>
            <li> <a href="#revision-history">Getting Started</a></li>
            <li> <a href="#revision-history">Public Methods</a></li>
            <li> <a href="#revision-history">Events</a></li>
            <li> <a href="#revision-history">Integration of System Check</a></li>
            <li> <a href="#revision-history">Integration of Verification Steps</a></li>
        </ol>
    </li>
    <li> 
        <a href="#revision-history">Auto Proctoring Report SDK</a> 
        <ol type="a">
            <li> <a href="#revision-history">Overview</a></li>
            <li> <a href="#revision-history">Sequence Diagram</a></li>
            <li> <a href="#revision-history">Getting Started</a></li>
            <li> <a href="#revision-history">Public Methods</a></li>
            <li> <a href="#revision-history">Integration of Report SDK</a></li>
        </ol>
    </li>
    <li> <a href="#revision-history">Error Handling</a> </li>
    <li> <a href="#revision-history">FAQs</a> </li>
</ol>

## Introduction

<p>
    The TestMaverick SDK offers a cutting-edge solution for effortless integration of AI-based proctoring into your LMS platforms. 
</p>

<p>
With its seamless and user-friendly approach, this software development kit empowers institutions and businesses to enhance the integrity of their assessments by leveraging advanced artificial intelligence technology. 
</p>

<p>
Whether you're an educational institution, a certification body, or a business seeking to maintain the highest standards of exam security, TestMaverick SDK provides a reliable and efficient means to ensure fair and transparent testing environments.
</p>

<p>
TestMaverick SDK provides following features:
</p>

<ul>
<li>Auto Proctoring</li>
<li>Proctoring Report</li>
<li>Manual Proctoring</li>
<li>Test player</li>
<li>Smart Test evaluation using ChatGPT</li>
<li>Plagiarism Check</li>
</ul>

<p>This document covers Auto Proctoring and Auto Proctoring Report SDK features.</p>

## Overview

<p>
Below is a top level architectural overview for integration of TestMaverick SDK with Consumer’s existing system. The Consumer system will integrate TestMavericks client SDK which will communicate with the Consumer system for data through events trigger and callbacks. Also TestMaverick SDK provides various public methods which can be accessed by consumers.
</p>

<p>
The TestMaverick SDK will store all the Proctoring related information on TestMaverick Server however it will not  store any PII or sensitive information of the user on our server. TestMaverick  SDK solely store statistical data related to auto proctoring (i.e Face, audio and browser related violation counts) along with few non-identifiable GUIDs like UserGUID, TestGUID and AttemptGUID, ensuring that no personal information is stored on our end. These identifiers, which are not directly linked to personal information, are essential for keeping the data mapping within the TestMaverick system so that when the TestMaverick Report SDK is loaded in your system, using these non-identifiable GUIDs it will accurately retrieve statistical information mapped to it from TestMaverick server.
</p>

<p align="center">
<img src="images/overview.png" alt="Overview arch" />
</p>

## Authentication and Security

<p>
TestMaverick SDK provides a signature based authentication and authorization system to integrate with various existing applications to ensure the security of our APIs. 
</p>
<p>
Following are the steps to be followed by Consumer to integrate SDK with the existing system.
</p>

<ol>
    <li>
        <strong>Registration and Configuration</strong>
        <p>
        To use the TestMaverick SDK, consumers must register with our service. Upon registering, they will receive a 'ClientID' and a 'Secret Key'. This set of secret keys needs to be kept confidential.</p>
    </li>
    <li>
        <strong>Add Signature API</strong>
        <p>
        Consumers must provide an API which can generate the signed object in the following output format.</p>
    </li>
</ol>
