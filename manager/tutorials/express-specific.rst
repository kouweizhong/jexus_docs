IIS Express Specific
====================

By `Lex Li`_

This page shows you how IIS Express works differently from other web servers.

.. contents:: In this article:
  :local:
  :depth: 1

Background
----------
IIS Express is a development server that behaves differently from full IIS.

Microsoft Documentation
-----------------------
Microsoft has listed several key differences in the article below,

http://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview

It might contain the key information but some users could not understand it easily.

Enable External Access
----------------------
IIS Express by default does not accept external requests. You can choose from the two approaches below to configure IIS Express to serve such requests.

Simplest Approach
^^^^^^^^^^^^^^^^^

#. Run Jexus Manager as administrator. Then IIS Express worker processes launched by it can hook to http.sys automatically.
#. Click the IIS Express server node you would like to manage.
#. Under Sites node, choose a site.
#. Click Bindings... menu item on Actions panel.
#. In Edit Bindings dialog, choose a binding that says "localhost" under "Host Name", and click Edit... button.
#. In Edit Site Binding dialog, remove the host header value under "Host name:", leave it blank, and click OK.
#. If a prompt says "The specific host name is not recommended for IIS Express...", click OK.
#. In Edit Bindings dialog, click Close.
#. Click Start menu item on Actions panel. This launches an IIS Express worker process with external access enabled.

Alternative Approach
^^^^^^^^^^^^^^^^^^^^
.. note:: This approach only requires one time elevation to configure reserved URLs.

#. Run Jexus Manager normally.
#. Click the IIS Express server node you would like to manage.
#. Click HTTP API icon in the middle.
#. Switched to Reserved URL tab.
#. Click Add... menu item on Actions panel.
#. In Add Reserved URL dialog, type the URL prefix (for example, "http://\*:8080/") and click OK. Elevation would be required.
#. Under Sites node, choose a site which has a site binding of "localhost on \*:8080 (http)".
#. Click Bindings... menu item on Actions panel.
#. In Edit Bindings dialog, choose a binding that says "localhost" under "Host Name", and click Edit... button.
#. In Edit Site Binding dialog, remove the host header value under "Host name:", leave it blank, and click OK.
#. If a prompt says "The specific host name is not recommended for IIS Express...", click OK.
#. In Edit Bindings dialog, click Close.
#. Click Start menu item on Actions panel. This launches an IIS Express worker process with external access enabled.

Enable Android Emulator Access
------------------------------
Sometimes application testing might requires mobile apps on Android Emulator to access web apps on IIS Express.

Simplest Approach
^^^^^^^^^^^^^^^^^
#. Run Jexus Manager normally.
#. Click the IIS Express server node you would like to manage.
#. Under Sites node, choose a site which would be exposed to Android Emulator (assume its binding is "localhost on \*:8080 (http)").
#. Click Bindings... menu item on Actions panel.
#. In Edit Bindings dialog, choose a binding that says "localhost" under "Host Name", and click Edit... button.
#. In Edit Site Binding dialog, replace the host header value under "Host name:" with "127.0.0.1" (without quotes), and click OK.
#. If a prompt says "The specific host name is not recommended for IIS Express...", click OK.
#. In Edit Bindings dialog, click Close.
#. Click Start menu item on Actions panel. This launches an IIS Express worker process.

As Google Android Emulator sends HTTP requests with host header set to 127.0.0.1, after changing the binding the web site should be 
accessible via http://10.0.2.2:8080 (Android Emulator forwards requests from 10.0.2.2 to the local IIS Express).

.. note:: Other Android Emulator might require some extra steps or special input.

Alternative Approach
^^^^^^^^^^^^^^^^^^^^
Follow "Enable External Access" section, and then the Android Emulator should be able to access the site.

Further Explanation
-------------------

Application Pools
^^^^^^^^^^^^^^^^^
Though IIS Express uses the same format of configuration file (``applicationHost.config``), and each web sites has an application pool assigned, it does not support 
application pool at all. Every time an IIS Express instance is launched, it always uses the current user's credentials, and ignore the identity setting in 
``applicationHost.config``.

As IIS Express is a single instance server, there is no application pool recycle either. You can manually stop and restart the process, but that's different from 
application pool recycle (which by default uses the overlapped mode).

This is what Microsoft means by saying "user launches and terminates sites".

Related Resources
-----------------

- :doc:`/getting-started/install`
- :doc:`/getting-started/features`
