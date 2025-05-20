- detect product, category follow a list defined by clients using bedrock


using
```
import {
  BedrockRuntimeClient,
  InvokeModelCommand,
} from "@aws-sdk/client-bedrock-runtime";

const client = new BedrockRuntimeClient({
  region: "us-east-1",
  credentials: {
    accessKeyId: "xxx",
    secretAccessKey: "xxx"
  },
});
```

RUN CODE 

```
const response = await axios.get(imageUrl, { responseType: "arraybuffer" });
    console.timeEnd("Image URL");

    const base64Data = Buffer.from(response.data, "binary").toString("base64");

    const input = {
      modelId: "anthropic.claude-3-5-sonnet-20240620-v1:0",
      contentType: "application/json",
      accept: "application/json",
      // body: JSON.stringify({
      //   prompt: appPrompt(imageUrl),
      //   max_tokens_to_sample: 256,
      // }),
      body: JSON.stringify({
        anthropic_version: "bedrock-2023-05-31",
        messages: [
          {
            role: "user",
            content: [
              {
                type: "image",
                source: {
                  type: "base64",
                  media_type: "image/jpeg",
                  data: base64Data,
                },
              },
              {
                type: "text",
                text: appPrompt(),
              },
            ],
          },
        ],
        max_tokens: 256,
      }),
    };

    try {
      const command = new InvokeModelCommand(input);
      const response = await client.send(command);
      const result = JSON.parse(new TextDecoder().decode(response.body));

      const completion = result.content.map((it: any) => {
        const json: any = parseJsonString(it.text);
        return json || {};
      });

      res.json({ completion });
    } catch (error: any) {
      res
        .status(500)
        .json({ error: "Claude API error", detail: error.message });
    }
```
