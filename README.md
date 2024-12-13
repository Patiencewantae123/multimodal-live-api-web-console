<p align="center">
  <img src="https://via.placeholder.com/150" alt="Multimodal Live API Logo" width="150"/>
</p>

<h1 align="center">Multimodal Live API - Web Console</h1>

<p align="center">
  <i>Your gateway to integrating multimodal interactions seamlessly.</i>
</p>

<p align="center">
  <a href="https://github.com/google-gemini/multimodal-live-api-web-console/actions">
    <img src="https://img.shields.io/github/actions/workflow/status/google-gemini/multimodal-live-api-web-console/ci.yml" alt="CI Status">
  </a>
  <a href="https://opensource.org/licenses/MIT">
    <img src="https://img.shields.io/badge/license-MIT-green" alt="License">
  </a>
  <a href="https://discord.gg/jgjmyYgHwB">
    <img src="https://img.shields.io/discord/1258658826257961020?color=7289da&logo=discord" alt="Discord">
  </a>
</p>

---

## 🌟 Features

- **Streamlined WebSocket Integration**: Quickly set up real-time multimodal applications.
- **Media Interaction Modules**: Record and stream from microphones, webcams, or screen captures.
- **Unified Development Logs**: Debug and monitor interactions seamlessly.

### Demo Applications
- 🌐 [GenExplainer](https://github.com/google-gemini/multimodal-live-api-web-console/tree/demos/genexplainer)
- ☁️ [GenWeather](https://github.com/google-gemini/multimodal-live-api-web-console/tree/demos/genweather)

---

## 📦 Example: Google Search & Altair Graph Rendering

```typescript
import { type FunctionDeclaration, SchemaType } from "@google/generative-ai";
import { useEffect, useRef, useState } from "react";
import vegaEmbed from "vega-embed";
import { useLiveAPIContext } from "../../contexts/LiveAPIContext";

export const declaration: FunctionDeclaration = {
  name: "render_altair",
  description: "Displays an altair graph in json format.",
  parameters: {
    type: SchemaType.OBJECT,
    properties: {
      json_graph: {
        type: SchemaType.STRING,
        description: "JSON STRING representation of the graph to render."
      }
    },
    required: ["json_graph"]
  }
};

export function Altair() {
  const [jsonString, setJSONString] = useState<string>("");
  const { client, setConfig } = useLiveAPIContext();

  useEffect(() => {
    setConfig({
      model: "models/gemini-2.0-flash-exp",
      systemInstruction: {
        parts: [
          {
            text: 'You are my helpful assistant. Any time I ask you for a graph call the "render_altair" function.'
          }
        ]
      },
      tools: [{ googleSearch: {} }, { functionDeclarations: [declaration] }]
    });
  }, [setConfig]);

  const embedRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (embedRef.current && jsonString) {
      vegaEmbed(embedRef.current, JSON.parse(jsonString));
    }
  }, [embedRef, jsonString]);

  return <div className="vega-embed" ref={embedRef} />;
}
```

---

## 🛠 Development

### Requirements
- **Node.js**: Install the latest LTS version.
- **Dependencies**: Run `npm install` to fetch all required packages.

### Available Scripts

#### `npm start`
Run the application in development mode. Open [http://localhost:3000](http://localhost:3000) to access it.

#### `npm run build`
Generate a production-ready build with optimized assets.

---

## 💡 FAQ

### What makes this different?
This project provides a developer-friendly toolkit to interact with Google's Multimodal Live API, featuring media streaming and easy API integrations.

### Example Use Cases
1. Dynamic weather updates using multimodal interactions.
2. Interactive graphs powered by Vega.
3. Real-time media recording applications.

---

## 📝 License

This repository is licensed under the [MIT License](LICENSE).

<p align="center">
  Made with ❤️ by the Multimodal API Team.
</p>

