---
layout: post
title:  "Cryptomator file and folders fix"
date:   2021-10-1 15:19:23
categories: cryptomator cyberduck
---

TLDR: Cryptomator does not work perfectly when using S3-like storage for small files and can fail pretty often in my experience. This is what fixed most of the issues: https://help.backblaze.com/hc/en-us/articles/217994508-Cyberduck-How-to-install-and-upload-a-file
In this link is described what needs to be changed in the Connection Settings to have more successful transfers.

If a file is not finished yet, Backblaze may be able to see that the file was not uploaded completely. Then it is visible in the bucket overviews and the path of the encrypted file can be deleted. Then the encrypted file is not visible in the Cryptomator vault anymore.

If a folder is not found anymore, the URL with the encrypted folder name can be deleted from Backblaze manually.

If the deleted folder is still visible in the Cryptomator vault, then renaming the folder is the only solution. After the folder is renamed the folder can be moved to another folder in the vault called 'faulty' for example. In that way the folders are not visible in the other folders anymore.


If the new folders with the new names are still prompted with 'File not found' then it is possible to move the folder to outside the Cryptomator vault (it is empty anyways, and it has a new name). Then that folder can be deleted????



The only good solution I found is to make small batches of uploading new files in one transaction in Cyberduck in using Cryptomator vaults.