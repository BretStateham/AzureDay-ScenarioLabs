<a name="Title" />
#Moving a Local Web Site's Images to Cloud Storage#

---

<a name="Overview">
##Overview##

In this lab, you will take an existing MVC 4 web application, and move it's images to cloud storage.  You will accomplish this by:

1. Setup existing web site and database locally. - Ensuring VS2012 Express Web installed and SQL Server 2012 Express.  Also create sample db in SQL Express MVC4Sample
1. Signing up for a Windows Azure account
2. Creating an Azure Storage Account
3. Adding an "images" container to the blob storage in the account
4. Granting public permissions to the "image" container
5. Upload the existing web site images into the new blob container
6. Deleting the original images from the website
7. Modifing the image urls in the site.css file to point to the images in blob storage.

