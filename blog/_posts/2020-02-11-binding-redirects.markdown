---
layout: post
author: Protik Das
title:  "Binding Redirects"
date:   2020-01-01
---

Test post

### Test header

Below is test code.

```xml
<configuration>
  <runtime>
    <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
      <dependentAssembly>
        <assemblyIdentity name="System.Runtime.CompilerServices.Unsafe" publicKeyToken="b03f5f7f11d50a3a" culture="neutral"/>
        <bindingRedirect oldVersion="0.0.0.0-4.0.6.0" newVersion="4.0.6.0"/>
      </dependentAssembly>
      <!-- ...and maybe some more... -->
    </assemblyBinding>
  </runtime>
</configuration>
```
