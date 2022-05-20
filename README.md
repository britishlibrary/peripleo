# What is *Peripleo*?

*Peripleo* is a browser-based tool for the mapping of things related to place. It was originally developed by partners in what has become the [Pelagios Network](https://pelagios.org/), and has now been revived as part of the [**Locating a National Collection project**](https://britishlibrary.github.io/locating-a-national-collection/home.html) (LaNC), primarily for the discovery and spatial visualisation of cultural heritage. It is hoped that this work will serve as a foundation for further development.

# Installation Guide

## What you need to get started

* Dataset(s) formatted as [Linked Places Format (LPF)](https://github.com/LinkedPasts/linked-places-format/blob/master/README.md) or GeoJSON. These are not complicated formats, and are described [here](https://github.com/britishlibrary/peripleo/blob/main/data-formats.md).
    * You can use any such dataset if it is accessible via a URL.
    * If your data is in a spreadsheet or delimited text (for example CSV), you will need to convert it using a tool such as [Locolligo](https://github.com/docuracy/Locolligo/blob/main/README.md).
* Somewhere to host and serve a simple HTML file, together with any datasets not hosted elsewhere. This guide begins with instructions for hosting your map on GitHub Pages: you might instead copy the files from the `docs` folder to another server.

## Setting up GitHub Pages

1. If you do not already have a GitHub account, go to https://github.com/signup and create one.
2. Sign in to GitHub.
3. Click on `Import Repository`.
4. Type `https://github.com/britishlibrary/peripleo` as your 'old repository's clone URL'.
5. Type a name for your new repository (perhaps related to the specifics of the map you are going to create), and decide whether to make it Public or Private.
6. Click on `Begin Import`, and wait for the process to complete.
7. Open your new repository either by clicking on the link shown when import completes, or by following the link to 'Your repositories' revealed by clicking on your name-badge icon, top-right.
8. Click on `Settings`.
9. Click on `Actions` and `General`.
10. In the 'Actions permissions' section, select `Allow all actions and reusable workflows` and click on `Save`.
11. Click on `Pages`.
12. In the 'Source' section, click on 'None' and select `Main`; next to that, select folder `/docs` in the drop-down list.
13. Click on `Save`.
14. The system will then give you the URL on which your site is published. **Please note that now and at every time you edit your site it may take a few minutes for it to be (re-)built and deployed before the changes are evident**. You can see progress by clicking on `Actions`. 
15. You can now check that *Peripleo* is running correctly with the default example configuration, or you can go right ahead and start reconfiguring it to display your own dataset(s).

## Editing Configuration Files on GitHub

1. Navigate to your repository and click on `Code`, then select the `Docs` folder.
2. Configuration is done principally in `peripleo.config.json`, but (optionally) also in `index.html`.
3. Click on a filename to see its contents, and then on the pencil icon to begin editing.
4. Configuration settings are described in detail in the [Configuration Guide](./Configuration-Guide.md). When you have finished editing each file, you need to type a *very* brief description of the changes you have made, and then click on `Commit changes`.
5. After a minute or so, you can check your modifications by going to your publication URL.

## Embedding Maps

Maps made using *Peripleo* can be embedded in other web sites, wikis, and blogs using IFrames. See [here](./Configuration-Guide.md#embedding-your-map) for some tips on how to do this.
