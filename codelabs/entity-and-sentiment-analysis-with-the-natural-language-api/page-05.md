# 5. Create an API Key

Since we'll be using curl to send a request to the Natural Language API, we'll need to generate an API key to pass in our request URL. To create an API key, navigate to the API Manager section of your project dashboard:

![Google Cloud Platform Image 1](https://codelabs.developers.google.com/codelabs/cloud-nl-intro/img/8cbae8dc9ba56e1e.png)

Then, navigate to the **Credentials** tab and click **Create credentials**:

![Credentials Image 1](https://codelabs.developers.google.com/codelabs/cloud-nl-intro/img/fc9b83db953a127a.png)

In the drop down menu, select **API key**:

![Dropdown Menu Image](https://codelabs.developers.google.com/codelabs/cloud-nl-intro/img/bc4940935c1bef7f.png)

Next, copy the key you just generated. You will need this key later in the lab.

Now that you have an API key, save it to an environment variable to avoid having to insert the value of your API key in each request. You can do this in Cloud Shell. Be sure to replace `<your_api_key>` with the key you just copied.

```
export API_KEY=<YOUR_API_KEY>
```