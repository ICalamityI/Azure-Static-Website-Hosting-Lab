# ☁️ Azure Static Website Hosting Lab

**Environment:** Windows · Azure Portal · Azure Blob Storage  
**Cloud Provider:** Microsoft Azure

---

## 📋 What This Lab Is

This is a hands-on Azure lab I completed to deploy my first public-facing cloud resource. Rather than provisioning a virtual machine or configuring a web server, I used Azure Blob Storage to host a static website — which is a core pattern in PaaS (Platform as a Service) and serverless architecture. The idea is that Azure handles all the infrastructure; you just provide the content.

It was a deliberately simple starting point, but the concepts here — resource groups, storage accounts, public endpoints, and serverless hosting — are the same building blocks used in production cloud environments every day.

---

## 🏗️ Architecture

```
User (Public Internet)
        │
        │  HTTPS Request
        │  Primary Endpoint URL
        ▼
┌──────────────────────────────────────────────────┐
│          rg-lab01  —  East US                    │
│                                                  │
│  ┌────────────────────────────────────────────┐  │
│  │        Azure Storage Account               │  │
│  │        stlab01  ·  Standard  ·  LRS        │  │
│  │                                            │  │
│  │  ┌──────────────────────────────────────┐  │  │
│  │  │   Static Website Hosting (Enabled)   │  │  │
│  │  │   Primary endpoint exposed           │  │  │
│  │  └───────────────────┬──────────────────┘  │  │
│  │                      │                     │  │
│  │  ┌───────────────────▼──────────────────┐  │  │
│  │  │   $web container                     │  │  │
│  │  │   └── index.html                     │  │  │
│  │  └──────────────────────────────────────┘  │  │
│  └────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────┘
```

When Static Website Hosting is enabled on a Storage Account, Azure automatically creates the `$web` container and exposes a public HTTPS endpoint. Any file uploaded to `$web` becomes accessible at that URL — no web server, no VM, no configuration beyond flipping the toggle.

---

## 🧠 Concepts I Applied

**Resource Groups** are logical containers in Azure that hold related resources together. Everything created in this lab lived inside one resource group, which made cleanup straightforward — deleting the group deletes everything inside it in one action.

**Azure Blob Storage** is Azure's object storage service, designed for storing unstructured data like images, documents, and — in this case — HTML files. It's not a web server, but when Static Website Hosting is enabled, Azure adds a public-facing layer on top that makes it behave like one for static content.

**PaaS vs IaaS** — this lab is a practical example of the PaaS model. With IaaS you'd provision a VM, install a web server like nginx or Apache, configure ports, manage OS updates, and handle availability yourself. With PaaS, all of that is abstracted away. The tradeoff is flexibility — PaaS is ideal for static content but wouldn't work for a dynamic application needing server-side logic.

**LRS (Locally-Redundant Storage)** replicates data three times within a single Azure datacenter. It's the cheapest redundancy tier and more than sufficient for a lab environment. Production workloads would typically use ZRS or GRS for higher resilience.

**Static Website Hosting** is the feature inside a Storage Account that exposes the `$web` container through a public HTTPS endpoint. The index document name (`index.html`) tells Azure which file to serve when someone hits the root URL. It's a zero-infrastructure way to serve static content globally.

---

## 🛠️ What I Did

### Phase 1 — Created the Resource Group

Logged into the [Azure Portal](https://portal.azure.com), searched for **Resource Groups** in the top search bar, and clicked **+ Create**. Set the name to `rg-lab01`, selected **(US) East US** as the region, and hit **Review + create → Create**.

The resource group is just a logical container at this point — nothing is running, nothing is costing money. It's the organizational boundary for everything that follows.

---

### Phase 2 — Created the Storage Account

Searched for **Storage accounts** in the portal, clicked **+ Create**, and filled in the basics:

- **Resource Group:** `rg-lab01`
- **Storage account name:** `stlab01` *(globally unique across all of Azure — lowercase letters and numbers only)*
- **Region:** East US
- **Performance:** Standard
- **Redundancy:** Locally-redundant storage (LRS)

Hit **Review + create**, waited for validation to pass, then clicked **Create**. Deployment finished in about 30 seconds. Clicked **Go to resource** to land on the Storage Account overview.

One thing worth noting: the storage account name has to be unique across *all* of Azure, not just your own subscription. If the name is taken, adding a few random numbers to the end resolves it.

---

### Phase 3 — Enabled Static Website Hosting

Inside the Storage Account, navigated to **Data management → Static website** in the left menu. Switched the toggle from **Disabled** to **Enabled**, set:

- **Index document name:** `index.html`
- **Error document path:** `404.html`

Clicked **Save**.

As soon as it saved, a **Primary endpoint URL** appeared — something like `https://stlab01.z13.web.core.windows.net/`. Copied that immediately. That URL is the public address of the website. Azure also automatically created the `$web` container in the storage account at this point.

---

### Phase 4 — Built the HTML File

Opened Notepad, pasted the following HTML, and saved the file to the Desktop as `index.html`:

```html
<!DOCTYPE html>
<html>
<head>
    <title>My First Cloud Site</title>
    <style>
        body { font-family: sans-serif; text-align: center; margin-top: 50px; background-color: #f0f0f0; }
        h1 { color: #0078d4; }
    </style>
</head>
<body>
    <h1>Hello from the Cloud!</h1>
    <p>This site is hosted on Azure Blob Storage.</p>
    <p>Deployed by: Jhante Charles</p>
</body>
</html>
```

The file name has to be exactly `index.html` — all lowercase. Azure is case-sensitive, so `Index.html` or `index.HTML` won't be served as the default document.

---

### Phase 5 — Uploaded to the $web Container

Back in the Azure Portal, navigated to **Data storage → Containers** in the left menu and opened the `$web` container that Azure had auto-created. Clicked **Upload**, browsed to the `index.html` file on the Desktop, and clicked **Upload**.

---

### Phase 6 — Validated the Deployment

Opened a new browser tab, pasted the primary endpoint URL, and hit Enter.

The page loaded with **"Hello from the Cloud!"** — a static HTML file sitting in Blob Storage, being served publicly over HTTPS with zero server configuration.

---

## 🧹 Clean Up

Deleted all resources by going to **Resource Groups → rg-lab01 → Delete resource group**, typed the name to confirm, and clicked **Delete**. Since everything was inside one resource group, that single action removed the storage account, the `$web` container, and the uploaded file in one step.

This is one of the practical benefits of using resource groups properly — cleanup is one action, not hunting down individual resources.

---

## 💡 What I Took Away

The most interesting part of this lab wasn't the HTML — it was understanding why this pattern exists. Hosting a static site on Blob Storage instead of a VM means there's no OS to patch, no web server process to manage, no scaling configuration to worry about. Azure handles all of it. You pay for storage and bandwidth, not compute.

The `$web` container being auto-created when you enable static hosting was a good example of how Azure abstracts setup steps. In the background, Azure is configuring access policies and routing rules — but from the portal side, it's just a toggle.

The LRS vs GRS distinction also clicked during this lab. For a public website where the content is already stored locally, LRS is perfectly fine. The moment uptime and data durability become critical — like a production app — you'd want ZRS or GRS to survive a datacenter failure.

---

## 📎 Resources

- [Azure Static Website Hosting — Official Docs](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-static-website)
- [Azure Storage Account Overview](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-overview)
- [Azure Free Tier](https://azure.microsoft.com/en-us/free/)
