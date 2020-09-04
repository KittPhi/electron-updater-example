> I update electron. it was `3.02`, I updated to `10.1.1`
> Update yarn `curl --compressed -o- -L https://yarnpkg.com/install.sh | bash`
> yarn
> yarn publish

This repo contains the **bare minimum code** to have an auto-updating Electron app using [`electron-updater`](https://github.com/electron-userland/electron-builder/tree/master/packages/electron-updater) with releases stored on a plain HTTP server.

This example uses `localhost` as the release server.

1. Install necessary dependencies with:

        ```python
        yarn
        # or
        npm install
        ```

2. Build your app with:

        ```python
        yarn run publish 
        # SAME AS: node_modules/.bin/build --linux --x64 --publish always
        ```

> If you want to publish for more platforms, edit the `publish` script in `package.json`.  For instance, to build for Windows and macOS:
        ```python
        "scripts": {
                "publish": "build --mac --win --linux --publish always"
        },
        ```
4. COPY the files in the `dist/` directory to the webserver directory `wwwroot`.  Here's how to do it on a Linux system:

        ```python
        mkdir -p wwwroot
        cp dist/*.yml wwwroot/
        cp dist/*.AppImage wwwroot/
        cp dist/*.deb wwwroot/
        ```

5. Serve `wwwroot` over HTTP:

        ```python
        node_modules/.bin/http-server wwwroot/ -p 8080
        ```

6. NOT WORKING: Download and Install the app from <http://127.0.0.1:8080>
> BUG_1: AppImage update works when getting it directly from `wwwroot` not the `http-server wwwroot/ -p 8080`

> BUG_2: deb file does not update, only AppImage.

7. Update the version in `package.json`.

8. Build your app AGAIN with the updated version:

        ```python
        yarn run publish 
        # SAME AS: node_modules/.bin/build --linux --x64 --publish always
        ```

9. COPY the files in the `dist/` directory to your webserver.  Here's how to do it on a Linux system:

        ```python
        cp dist/*.yml wwwroot/ # notice that the .yml file is overwritten
        cp dist/*.AppImage wwwroot/
        cp dist/*.deb wwwroot/
        ```

10. OPEN (first time) the INSTALLED VERSION of the app and SEE that it updates itself.
- It will take a couple minutes to Render
