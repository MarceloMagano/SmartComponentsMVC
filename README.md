# SmartComponentsMVC
Trying out the new .Net smart components powered by AI



## Adding the Smart Components
Create new project dotnet `new mvc --name SmartComponentsMVC`

Adding the prerelease package with the components `dotnet add package --prerelease SmartComponents.AspNetCore`

Adding the `AddSmartComponents` to the **Program.cs**

```
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddControllersWithViews();
builder.Services.AddSmartComponents();
```

Reference the Smart Component tag helpers by adding `@addTagHelper *, SmartComponents.AspNetCore` to the **_ViewImports.cshtml**

```
@using SmartComponentsMVC
@using SmartComponentsMVC.Models
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper *, SmartComponents.AspNetCore
```

Add the Smart Text Area, we can tweak the component to a certain context by defining the **user role**

```
<div>
  <smart-textarea user-role="An IT servicedesk employee handling IT related problems"></smart-textarea>
</div>
```

Can also help the language model reply using prefered tone, common phares and more

```
@{
    ViewData["Title"] = "Home Page";

    string[] userPhrases = new string[]
    {
      "Did you try turning it off and on again?",
      "Sure, I can help you reset your password. Could you please verify your username and answer your security questions?",
      "Of course, I can guide you through installing the software. Let me walk you through the process step by step.",
      "No problem, I can assist you in setting up your email. Could you please provide the email client you're using?",
      "I see you're experiencing issues with the printer. Let's check if it's properly connected and has enough paper and toner.",
      "I understand you're receiving an error message. Could you please provide the exact wording of the error, and I'll help you troubleshoot it?",
      "I'm here to assist you with any technical issues you're facing. Please describe the problem you're encountering, and I'll do my best to help."
    };
}
<div class="text-center">
    <h1 class="display-4">Welcome</h1>
</div>

<div>
    <smart-textarea user-role="An IT servicedesk employee handling IT related problems" user-phrases="userPhrases" rows="10" cols="80"/>
</div>
```

## Using self hosted AI backend

#### Can use the [**Ollama**](https://ollama.com/) to run LLM locally, i used the **Phi-3 Mini**

Download and install the application

Run LLM `ollama run phi3`

Add to your `appsettings.json`, the **DeploymentName** needs to match the model used

```
"SmartComponents": {
    "SelfHosted": true,
    "DeploymentName": "phi3",
    "Endpoint": "http://localhost:11434/"
  }
```

Since Ollama exposes an OpenAI compatible API you need to install the another package `dotnet add package --prerelease SmartComponents.Inference.OpenAI`

And finally register it inside **Program.cs** by adding `WithInferenceBackend<OpenAIInferenceBackend>()`
```
builder.Services.AddSmartComponents().WithInferenceBackend<OpenAIInferenceBackend>();
```
