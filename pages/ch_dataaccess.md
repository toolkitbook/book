---
layout: default
title: C++ Toolkit test
nav: pages/ch_dataaccess
---

17. Access to NCBI data
=======================

Created: January 26, 2009.

Overview
--------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

This chapter describes access to the NCBI data using the NCBI C++ Toolkit.

### Chapter Outline

-   [Object Manager: Generic API for retrieving and manipulating biological sequence data](#ch-dataaccess.Object-Manager)

-   [E-Utils: Access to Entrez Data](#ch-dataaccess.EUtils-Access-to-Ent)

<a name="ch-dataaccess.Object-Manager"></a>

Object Manager: Generic API for retrieving and manipulating biological sequence data
------------------------------------------------------------------------------------

The information about Object Manager library is [here](ch_objmgr.html).

<a name="ch-dataaccess.EUtils-Access-to-Ent"></a>

E-Utils: Access to Entrez Data
------------------------------

<a name="ch-dataaccess.EUtils-requests"></a>

### EUtils requests

The base class for all requests is [CEUtils\_Request](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CEUtils_Request). Derived request classes provide *Get/Set* methods to specify arguments for each request. The returned data can be read in several ways:

-   ******Read()****** - reads the data returned by the server into a string.

-   ******GetStream()****** - allows to read plain data returned by the server.

-   ******GetObjectIStream()****** - returns serial stream for reading data (in most cases it's an XML stream).

<a name="ch-dataaccess.Connection-context"></a>

### Connection context

[CEUtils\_ConnContext](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CEUtils_ConnContext) allows transferring EUtils context from one request to another. It includes user-provided information (tool, email) and history data (WebEnv, query\_key). If no context is provided for a request (the *ctx* argument is *NULL*), a temporary context will be created while executing the request.

<a name="ch-dataaccess.EUtils-objects"></a>

### EUtils objects

Most requests return specific data types described in EUtils DTDs. The C++ classes generated from the DTDs can be found in [include/objtools/eutils/](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/objtools/eutils)`<util-name>`.

<a name="ch-dataaccess.Sample-application"></a>

### Sample application

An example of using EUtils API can be found in [sample/app/eutils/eutils\_sample.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/eutils/eutils_sample.cpp).


