Plugins (Beta)
===============

.. note::
  This is the admin documentation for plugins. If you're a developer looking to build a plugin, see `our developer documentation <https://developers.mattermost.com/extend/plugins>`__.

Mattermost supports plugins to customize and extend the platform. See our `demo plugin <https://github.com/mattermost/mattermost-plugin-demo>`__ that illustrates the potential for a Mattermost plugin. To start writing your own plugin, consult our `starter template <https://github.com/mattermost/mattermost-plugin-starter-template>`__.

Consider using a plugin in the following scenarios:

 - When you want to customize the Mattermost user interface.
 - When you want to extend Mattermost functionality to meet a specific, complex requirement.
 - When you want to build integrations that are managed by your Mattermost server.

Plugins are fully supported in both Team and Enterprise Editions. 

Plugin Marketplace
------------------
The Plugin Marketplace is a collection of plugins that can greatly increase the value and capabilities of your Mattermost deployment. End users can see increased productivity through quick access to systems such as Jira, Zoom, Webex, and GitHub.  Mattermost Admins can discover new plugins and quickly deploy them to their servers, including High Availability clusters. The Marketplace is available in 5.16 and accessed via **Main Menu > Plugin Marketplace**. The Marketplace listings will continue to expand as we add new features and plugins that our customers request.  

.. image:: https://user-images.githubusercontent.com/915956/66891467-1b18eb80-ef9e-11e9-9de3-37a3c5899bd8.png

Installing a Plugin
~~~~~~~~~~~~~~~~

When a new plugin becomes available on the Marketplace, it's listed with an option to **Install**. Select **Install** to download and install the latest plugin binary from its respective GitHub repository. If there's a cluster present, the plugin will be distributed to each server automatically.   

Configuring and Enabling a Plugin
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Once a plugin is installed (or pre-installed if shipped with Mattermost binary release):

1. Select **Configure** to open the **Settings** menu.  
2. Enter the Plugin settings as required.
3. Set **Enable Plugin** setting to ``True``. If this flag is not enabled, the plugin will not become active.
4. Test out the plugin as needed.


Upgrading Plugins
~~~~~~~~~~~~~~~~~

In v5.16, the Marketplace only supports the installation of new plugins. To upgrade a plugin, you need to manually update it by downloading the binary file from the GitHub repository and upload it in **System Console > Plugin management**. In v5.18 (Dec 2019), you'll be able to upgrade a plugin on demand when a new version becomes available.


Marketplace Server
------------------

There are two Marketplace settings in **System Console > Plugin management**:

.. image:: https://user-images.githubusercontent.com/915956/66892854-94660d80-efa1-11e9-805c-a85223d43a07.png

- **Enable Marketplace**. Turns the Marketplace user interface on or off for all users (including System Admins).
- **Marketplace URL**. The location of the Marketplace server to query for new plugins. Mattermost hosts a Plugin Marketplace for the community and this is the default value for this field. You can also set up your own Marketplace server.

When you first access the Marketplace, your Mattermost server will attempt to contact the Mattermost Marketplace server and return a list of available plugins that are appropriate based on the server version that is currently running. Only your server version and search query is passed over to Mattermost Marketplace; we retain an anonymized record for product analytics whenever a new plugin is installed, unless you have opted out of `Telemetry <https://docs.mattermost.com/administration/telemetry.html>`__. The `Plugin marketplace server code <https://github.com/mattermost/mattermost-marketplace>`__ is available as an open source project and can be used to setup your own private Marketplace if desired.  


Mattermost Marketplace
~~~~~~~~~~~~~~~~~~~~~~~

The `Mattermost Plugin Marketplace <https://github.com/mattermost/mattermost-marketplace>`__ is a service run by Mattermost that contains listings of plugins that we have reviewed and, in many cases, built. In the future, we plan to include community-developed plugins that will be labeled separately to Mattermost-developed plugins. as well as settings that would restrict which types of plugins you can install. Comments in our forum are welcome as we develop this feature further. 

Mattermost Integration Directory
--------------------------------

There are many ways to integrate Mattermost aside from plugins, and we have created a directory of integration "recipes", some of which are scripts, plugins, or instructions on how to connect Mattermost with your Enterprise systems. Many are sourced from our community of customers. You can browse the directory at `https://integrations.mattermost.com/ <https://integrations.mattermost.com/>`__.


About Plugins
-------------
Plugins may have one or both of the following parts:

 - **Webapp plugins**. Customize the Mattermost user interface by adding buttons to the channel header, overriding the ``RHS``, or even rendering a custom post type within the center channel. All this is possible without having to fork the source code and rebase on every Mattermost release. For a sample plugin, see `our Zoom plugin <https://github.com/mattermost/mattermost-plugin-zoom>`__.
 - **Server plugins**. Run a Go process alongside the server, filtering messages, or integrating with third-party systems such as Jira, GitLab or Jenkins. For a sample plugin, see `our Jira plugin <https://github.com/mattermost/mattermost-plugin-jira>`__.



Security
--------
Plugins are intentionally powerful and not artificially sandboxed in any way and effectively become part of the Mattermost server. Server plugins can execute arbitrary code alongside your server and webapp plugins can deploy arbitrary code in client browsers. 

While this power enables deep customization and integration, it can be abused in the wrong hands. Plugins have full access to your server configuration and thus also to your Mattermost database. Plugins can read any message in any channel, or perform any action on behalf of any user in the webapp.

You should only install custom plugins from sources you trust to avoid compromising the security of your installation.

Set Up Guide
------------

To manage plugins, go to **System Console > Plugins > Plugin Management**. From here, you can:

 - Enable or disable pre-packaged plugins.
 - Install and manage custom plugins.

.. Note::
  In versions prior to 5.16, go to **System Console > Plugins (Beta) > Configuration**.


Pre-packaged Plugins
~~~~~~~~~~~~~~~~~~~
Mattermost ships with a number of pre-packaged plugins written and maintained by Mattermost. Instead of building these features directly into the product, you can selectively enable the functionality your installation requires. Pre-packaged plugins cannot be removed via the System Console, but can be customized by modifying the ``prepackaged_plugins`` directory in your Mattermost installation.

Custom Plugins
~~~~~~~~~~~~~~
Installing a custom plugin introduces some risk. As a result, plugin uploads are disabled by default and cannot be enabled via the System Console or REST API.

To enable plugin uploads, manually set **PluginSettings > EnableUploads** to ``true`` in your configuration and restart your server. You can disable plugin uploads at any time without affecting previously uploaded plugins.

With plugin uploads enabled, navigate to **System Console > Plugins > Management** and upload a plugin bundle. Plugin bundles are ``*.tar.gz`` files containing the server executables and webapp resources for the plugin. You can also specify a URL to install a plugin bundle from a remote source.

Custom plugins may also be installed via the `command line interface <https://docs.mattermost.com/administration/command-line-tools.html#mattermost-plugin>`__.

While no longer recommended, plugins may also be installed manually by unpacking the plugin bundle inside the `plugins` directory of a Mattermost installation.

Plugin Uploads in High Availability Mode
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Prior to Mattermost 5.14, Mattermost servers configured for `High Availability mode <https://docs.mattermost.com/deployment/cluster.html>`_ required plugins to be installed manually. As of Mattermost 5.14, plugins uploaded via the System Console or the command line interface are persisted to the configured file store and automatically installed on all servers that join the cluster. 

Manually installed plugins remain supported, but must be individually installed on each server in the cluster.

Frequently Asked Questions (FAQs)
---------------------------------

Where Can I Share Feedback on Plugins?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Join our community server discussion in the `Toolkit channel <https://community.mattermost.com/core/channels/developer-toolkit>`__.

Troubleshooting
-----------------

Plugin Uploads Fail Even Though Uploads are Enabled
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If plugin uploads fail and you see "permission denied" errors in **System Console > Logs** such as:

.. code-block:: text

  [2017/11/13 20:42:18 UTC] [EROR] failed to start up plugins: mkdir /home/ubuntu/mattermost/client/plugins: permission denied

the Mattermost server doesn't have the necessary permissions for uploading plugins. Ensure the Mattermost server has write access to the ``mattermost/client`` directory.

It may also be that the working directory for the service running Mattermost is not correct. On Ubuntu you might see

.. code-block:: text

    [2018/01/03 08:34:47 EST] [EROR] failed to start up plugins: mkdir ./client/plugins: no such file or directory

This can be fixed on Ubuntu 16.04 and RHEL by opening the service configuration file and setting ``WorkingDirectory`` to the path to Mattermost (generally it's ``/opt/mattermost``).

A similar problem can occur on Windows:

.. code-block:: text

    [EROR] failed to start up plugins: mkdir ./client/plugins: The system cannot find the path specified.

To fix this, set the ``AppDirectory`` of your service using ``nssm set mattermost AppDirectory c:\mattermost``.

``x509: certificate signed by unknown authority``
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you're seeing ``x509: certificate signed by unknown authority`` in your server logs, it usually means that the CA for a self-signed certificate for a server your plugin is trying to access has not been added to your local trust store of the machine the Mattermost server is running on.

You can add one in Linux `following instructions in this StackExchange article <https://unix.stackexchange.com/questions/90450/adding-a-self-signed-certificate-to-the-trusted-list>`_, or set up a load balancer like NGINX per :doc:`production install guide <config-ssl-http2-nginx>` to resolve the issue.
