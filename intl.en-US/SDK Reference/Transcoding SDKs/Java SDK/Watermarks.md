# Watermarks {#concept_o1j_ncx_y2b .concept}

1.  Create AcsClient instance.

    ```
    DefaultProfile profile = DefaultProfile.getProfile(
                    mpsRegionId,      // Region ID
                    accessKeyId,      // AccessKey ID
                    accessKeySecret); // Access Key Secret
    IAcsClient client = new DefaultAcsClient(profile);
    ```

2.  Create request, and set parameters.

    ```
    SubmitJobsRequest request = new SubmitJobsRequest();
    ```

3.  Set transcoding parameters.
    -   Image watermark

        ```
        // Image Watermark
           JSONObject imageWatermarkInput = new JSONObject();
           imageWatermarkInput.put("Location", ossLocation);
           imageWatermarkInput.put("Bucket", ossBucket);
           try {
               imageWatermarkInput.put("Object", URLEncoder.encode(imageWatermarkObject, "utf-8"));
           } catch (UnsupportedEncodingException e) {
               throw new RuntimeException("imageWatetmark Input URL encode failed");
           }
           JSONObject imageWatermark = new JSONObject();
           imageWatermark.put("WaterMarkTemplateId", watermarkTemplateId);
           imageWatermark.put("Type", "Image");
           imageWatermark.put("InputFile", imageWatermarkInput);
           imageWatermark.put("ReferPos", "TopRight");
           imageWatermark.put("Width", "0.05");
           imageWatermark.put("Dx", "0");
           imageWatermark.put("Dy", "0");
        ```

    -   Text watermark

        ```
        // Text Watermark
           JSONObject textConfig = new JSONObject();
           textConfig.put("Content", "5rWL6K+V5paH5a2X5rC05Y2w");
           textConfig.put("FontName", "SimSun");
           textConfig.put("FontSize", "16");
           textConfig.put("FontColor", "Red");
           textConfig.put("FontAlpha", "0.5");
           textConfig.put("Top", "10");
           textConfig.put("Left", "10");
           JSONObject textWatermark = new JSONObject();
           textWatermark.put("WaterMarkTemplateId", watermarkTemplateId);
           textWatermark.put("Type", "Text");
           textWatermark.put("TextWaterMark", textConfig.toJSONString());
        ```

    -   Video watermark

        ```
        // Video Watermark
           JSONObject videoWatermarkInput = new JSONObject();
           videoWatermarkInput.put("Location", ossLocation);
           videoWatermarkInput.put("Bucket", ossBucket);
           try {
               videoWatermarkInput.put("Object", URLEncoder.encode(videoWatermarkObject, "utf-8"));
           } catch (UnsupportedEncodingException e) {
               throw new RuntimeException("videoWatetmark Input URL encode failed");
           }
           JSONObject videoWatermark = new JSONObject();
           videoWatermark.put("WaterMarkTemplateId", watermarkTemplateId);
           videoWatermark.put("Type", "Image");
           videoWatermark.put("InputFile", videoWatermarkInput);
           videoWatermark.put("ReferPos", "BottomLeft");
           videoWatermark.put("Height", "240");
           videoWatermark.put("Dx", "0");
           videoWatermark.put("Dy", "0");
        ```

4.  Initiate API request and display returned value.

    ```
    SubmitJobsResponse response;
    response = client.getAcsResponse(request);
    System.out.println("RequestId is:"+response.getRequestId());
    if (response.getJobResultList().get(0).getSuccess()) {
        System.out.println("JobId is:" + response.getJobResultList().get(0).getJob().getJobId());
    } else {
        System.out.println("SubmitJobs Failed code:" + response.getJobResultList().get(0).getCode() +
                           " message:" + response.getJobResultList().get(0).getMessage());
    }
    ```


Full codes

```
package com.aliyun.mts;
import java.io.UnsupportedEncodingException;
import java.net.URLEncoder;
import com.alibaba.fastjson.JSONArray;
import com.alibaba.fastjson.JSONObject;
import com.aliyuncs.profile.DefaultProfile;
import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.IAcsClient;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.exceptions.ServerException;
import com.aliyuncs.mts.model.v20140618.*;
public class Watermark {
    private static String accessKeyId = "xxx";
    private static String accessKeySecret = "xxx";
    private static String mpsRegionId = "cn-hangzhou";
    private static String pipelineId = "xxx";
    private static String watermarkTemplateId = "xxx";
    private static String templateId = "S00000001-200030";
    private static String ossLocation = "oss-cn-hangzhou";
    private static String ossBucket = "presigned";
    private static String ossInputObject = "input.mp4";
    private static String ossOutputObject = "output.mp4";
    private static String imageWatermarkObject = "logo.png";
    private static String videoWatermarkObject = "logo.mov";
    public static void main(String[] args) {
        // DefaultAcsClient
        DefaultProfile profile = DefaultProfile.getProfile(
                mpsRegionId,      // Region ID
                accessKeyId,      // AccessKey ID
                accessKeySecret); // Access Key Secret
        IAcsClient client = new DefaultAcsClient(profile);
        // request
        SubmitJobsRequest request = new SubmitJobsRequest();
        // Input
        JSONObject input = new JSONObject();
        input.put("Location", ossLocation);
        input.put("Bucket", ossBucket);
        try {
            input.put("Object", URLEncoder.encode(ossInputObject, "utf-8"));
        } catch (UnsupportedEncodingException e) {
            throw new RuntimeException("input URL encode failed");
        }
        request.setInput(input.toJSONString());
        // Output
        String outputOSSObject;
        try {
            outputOSSObject = URLEncoder.encode(ossOutputObject, "utf-8");
        } catch (UnsupportedEncodingException e) {
            throw new RuntimeException("output URL encode failed");
        }
        JSONObject output = new JSONObject();
        output.put("OutputObject", outputOSSObject);
        // Ouput->TemplateId
        output.put("TemplateId", templateId);
        // Image Watermark
        JSONObject imageWatermarkInput = new JSONObject();
        imageWatermarkInput.put("Location", ossLocation);
        imageWatermarkInput.put("Bucket", ossBucket);
        try {
            imageWatermarkInput.put("Object", URLEncoder.encode(imageWatermarkObject, "utf-8"));
        } catch (UnsupportedEncodingException e) {
            throw new RuntimeException("imageWatetmark Input URL encode failed");
        }
        JSONObject imageWatermark = new JSONObject();
        imageWatermark.put("WaterMarkTemplateId", watermarkTemplateId);
        imageWatermark.put("Type", "Image");
        imageWatermark.put("InputFile", imageWatermarkInput);
        imageWatermark.put("ReferPos", "TopRight");
        imageWatermark.put("Width", "0.05");
        imageWatermark.put("Dx", "0");
        imageWatermark.put("Dy", "0");
        // Text Watermark
        JSONObject textConfig = new JSONObject();
        textConfig.put("Content", "5rWL6K+V5paH5a2X5rC05Y2w");
        textConfig.put("FontName", "SimSun");
        textConfig.put("FontSize", "16");
        textConfig.put("FontColor", "Red");
        textConfig.put("FontAlpha", "0.5");
        textConfig.put("Top", "10");
        textConfig.put("Left", "10");
        JSONObject textWatermark = new JSONObject();
        textWatermark.put("WaterMarkTemplateId", watermarkTemplateId);
        textWatermark.put("Type", "Text");
        textWatermark.put("TextWaterMark", textConfig.toJSONString());
        // Video Watermark
        JSONObject videoWatermarkInput = new JSONObject();
        videoWatermarkInput.put("Location", ossLocation);
        videoWatermarkInput.put("Bucket", ossBucket);
        try {
            videoWatermarkInput.put("Object", URLEncoder.encode(videoWatermarkObject, "utf-8"));
        } catch (UnsupportedEncodingException e) {
            throw new RuntimeException("videoWatetmark Input URL encode failed");
        }
        JSONObject videoWatermark = new JSONObject();
        videoWatermark.put("WaterMarkTemplateId", watermarkTemplateId);
        videoWatermark.put("Type", "Image");
        videoWatermark.put("InputFile", videoWatermarkInput);
        videoWatermark.put("ReferPos", "BottomLeft");
        videoWatermark.put("Height", "240");
        videoWatermark.put("Dx", "0");
        videoWatermark.put("Dy", "0");
        // Output->Watermarks
        JSONArray watermarks = new JSONArray();
        watermarks.add(imageWatermark);
        watermarks.add(textWatermark);
        watermarks.add(videoWatermark);
        output.put("WaterMarks", watermarks.toJSONString());
        // Outputs
        JSONArray outputs = new JSONArray();
        outputs.add(output);
        request.setOutputs(outputs.toJSONString());
        request.setOutputBucket(ossBucket);
        request.setOutputLocation(ossLocation);
        // PipelineId
        request.setPipelineId(pipelineId);
        // call api
        SubmitJobsResponse response;
        try {
            response = client.getAcsResponse(request);
            System.out.println("RequestId is:"+response.getRequestId());
            if (response.getJobResultList().get(0).getSuccess()) {
                System.out.println("JobId is:" + response.getJobResultList().get(0).getJob().getJobId());
            } else {
                System.out.println("SubmitJobs Failed code:" + response.getJobResultList().get(0).getCode() +
                                   " message:" + response.getJobResultList().get(0).getMessage());
            }
        } catch (ServerException e) {
            e.printStackTrace();
        } catch (ClientException e) {
            e.printStackTrace();
        }
    }
}
```

