---
title: "Resource interpreted as Stylesheet but transferred with MIME type application/xml"
permalink: /aws/04-resource-interpreted-as-stylesheet-but-transferred.html
excerpt: "On AWS"
header:
  overlay_image: /assets/images/aws/05-resource-interpreted-as-stylesheet-but-transferred/resource-interpretted-as-stylesheet.jpg
  teaser: /assets/images/aws/05-resource-interpreted-as-stylesheet-but-transferred/resource-interpretted-as-stylesheet.jpg
  overlay_filter: 0.5
last_modified_at: 2018-08-05T15:59:07-04:00
toc: true
author: Jay
---
If you receive this error in S3 hosted style sheets

1. Navigate to the file in overview tab
2. Go to Properties->Metadata
3. Set or add the key 'Content-Type' and the value `text/css`

{% include figure image_path="assets/images/aws/05-resource-interpreted-as-stylesheet-but-transferred/metadata.png" alt="this is a placeholder image" caption="Content-Type : text/css" %}