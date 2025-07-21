

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








