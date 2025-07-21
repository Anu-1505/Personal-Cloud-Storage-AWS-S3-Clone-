# Personal-Cloud-Storage-AWS-S3-Clone-
Designed and developed a secure, scalable web application that mimics the core functionalities of a cloud storage service like AWS S3. The application allows for user authentication, file uploads, downloads, and secure object storage. 
# Personal Cloud Storage

A web-based personal cloud storage application built with Python and AWS. This project allows users to securely upload, store, access, and manage their files from anywhere, demonstrating core concepts of cloud application development.

## About The Project

This application provides a simplified, personal alternative to commercial cloud storage services. It's designed to showcase proficiency in fundamental cloud technologies, particularly within the Amazon Web Services (AWS) ecosystem.

### Key Features:
*   **User Authentication:** Secure user sign-up and login functionality.
*   **File Operations:** Users can upload, download, and delete their files.
*   **Secure Storage:** Leverages AWS S3 for durable and scalable object storage.
*   **Web Interface:** A clean and simple front-end for easy file management.

### Built With

*   [Python (Flask)](https://flask.palletsprojects.com/) - Web Framework
*   [Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) - AWS SDK for Python
*   [AWS S3](https://aws.amazon.com/s3/) - Scalable Cloud Storage
*   [AWS EC2](https://aws.amazon.com/ec2/) - Virtual Server Hosting
*   [HTML/CSS](https://developer.mozilla.org/en-US/docs/Web/HTML) - Front-End


import software.amazon.awssdk.auth.credentials.ProfileCredentialsProvider;
import software.amazon.awssdk.regions.Region;
import software.amazon.awssdk.services.s3.S3Client;
import software.amazon.awssdk.services.s3.model.PutObjectRequest;
import software.amazon.awssdk.core.sync.RequestBody;

import java.io.File;

public class S3Uploader {

    public static void main(String[] args) {
        // Define the AWS region, bucket name, and file details
        Region region = Region.US_EAST_1; // e.g., Region.US_EAST_1
        String bucketName = "your-unique-bucket-name";
        String keyName = "my-uploaded-file.txt"; // The name of the file in the S3 bucket
        String filePath = "path/to/your/local/file.txt"; // The path to the file on your computer

        // Create an S3 client
        S3Client s3 = S3Client.builder()
                .region(region)
                .credentialsProvider(ProfileCredentialsProvider.create())
                .build();

        // Call the upload method
        uploadFile(s3, bucketName, keyName, filePath);

        // Close the S3 client
        s3.close();
    }

    /**
     * Uploads a file to an S3 bucket.
     *
     * @param s3         The S3 client instance.
     * @param bucketName The name of the S3 bucket.
     * @param keyName    The name for the object in S3.
     * @param filePath   The path to the local file to upload.
     */
    public static void uploadFile(S3Client s3, String bucketName, String keyName, String filePath) {
        try {
            PutObjectRequest putObjectRequest = PutObjectRequest.builder()
                    .bucket(bucketName)
                    .key(keyName)
                    .build();

            // Upload the file
            s3.putObject(putObjectRequest, RequestBody.fromFile(new File(filePath)));

            System.out.println("Successfully uploaded " + keyName + " to " + bucketName);

        } catch (Exception e) {
            System.err.println("Error in uploading file: " + e.getMessage());
            e.printStackTrace();
        }
    }
}








