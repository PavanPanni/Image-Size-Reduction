In this project, I built an image-processing workflow using Azure Functions.
The main idea is to upload an image through an HTTP API, push a message into an Azure Storage Queue, and then let a Queue Trigger Function resize the image and store the outputs in Blob Storage.

Overview:
1)This solution uses:
2)HTTP Trigger Function to upload images
3)Azure Storage Blob to store original and resized images
4)Azure Storage Queue to hold resize jobs
5)Queue Trigger Function to process each job
6)Pillow (PIL) for image resizing
7)The flow is completely serverless.

How the System Works:
1)I created an HTTP API endpoint (POST /api/upload) where the user uploads an image.
2)The function saves the original image into the uploads container.
3)After saving, it sends a queue message to image-jobs with
4)A Queue Trigger Function reads this message.
5)It downloads the original image, resizes it to 320 px and 1024 px, and uploads the results to the resized container inside separate folders.
6)After processing, it creates a JSON log in function-logs/ImageResizer/<date>/ containing:
7)original URL
8)resized URLs
9)processing time

Status:
Any failures are retried automatically.
If it fails more than 5 times, the message goes to dead-letter handling.

Blob Containers I Used:
uploads → stores original uploaded images
resized → stores the resized outputs (320, 1024)
function-logs → stores JSON logs for each processed message

Workflow Summary:
1)User uploads an image via HTTP.
2)Image is stored in Blob Storage.
3)A queue message is created.
4)Queue Trigger processes the message.
5)Resized images are saved.
6)JSON logs are written for each job.

What I Tested:
1)Upload API successfully saves the file and adds queue messages.
2)Queue Trigger resizes correctly into both formats.
3)Log file creation works with proper details.







