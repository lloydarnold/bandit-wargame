# Natas -- Stage by stage explanation

## Level 0 -> 1
After initially logging in to natas 0, by going to address `http://natas0.natas.labs.overthewire.org` and entering natas0 for both fields, we are greeted by a page which says "You can find the password for the next level on this page". 
A quick right click -> inspect element reveals the password as an HTML comment.

The password for natas 1 is ` gtVrDuiDfck831PqWsLEZy5gyDz1clto `

## Level 1 -> 2
Again, we are told that the password is somewhere on the page, but right clicking has been blocked this time. No bother; we can simply hit f12 to pull up the developer pane. 
The password is stored as an HTML comment again.

The password for natas 2 is ` ZluruAthQk7Q2MqmDeTiUij2ZvWy2mBi `

## Level 2 -> 3
Here, we are told that there is nothing on this page. A quick inspect element is, at first, promising - it reveals a .png file.
However, this file seems to just be a single white pixel, and is of no use. We can see that it's stored in the /files folder - navigating to this (by adding /files/ to the end of the url) we find a file called users.txt.
This contains the password for natas3.

The password for natas 3 is ` sJIJNW6ucpu6HPZ1ZAchaDtwd7oGrD14 `

## Level 3 -> 4
As before, we are told that there is nothing on this page. A quick inspect element reveals to us an HTML comment, `<!-- No more information leaks!! Not even Google will find it this time... -->`.
This suggests that this might have something to do with the robots.txt file; these exist in websites to tell a search engine to ignore certain pages.
Sure enough, navigating to this file, we find out that natas3 wants google to ignore the file `/s3cr3t/`. Navigating here (again, via URL) we are greeted once more with the users.txt file.

The password for natas4 is ` Z9tkRkWmpt9Qr7XrR5jWRkgOU901swEZ `

## Level 4 -> 5

