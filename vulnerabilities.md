---
layout: default
title: Vulnerabilities
permalink: /vulnerabilities/
---

<a href="{{ '/' | relative_url }}">{{ site.theme_config.back_home_text }}</a>

# Vulnerabilities

A running list of vulnerabilities I have discovered and their associated CVEs.

<div class="vuln-list">
  <article class="vuln-entry">
    <a class="vuln-cve" href="https://www.amd.com/en/resources/product-security/bulletin/amd-sb-9022.html">CVE-2025-61969</a>
    <span class="vuln-summary">Privileged file write leading to LPE in <code>AMDPowerProfiler.sys</code></span>
    <a class="vuln-writeup" href="{% post_url 2026-02-10-AMDuProf-1 %}">Writeup</a>
  </article>

  <article class="vuln-entry">
    <a class="vuln-cve" href="https://www.amd.com/en/resources/product-security/bulletin/amd-sb-9025.html">CVE-2026-0466</a>
    <span class="vuln-summary">AMD uProf kernel-shared memory write leading to crash or DoS</span>
    <span class="vuln-writeup"></span>
  </article>

  <article class="vuln-entry">
    <a class="vuln-cve" href="https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-49162">CVE-2026-49162</a>
    <span class="vuln-summary">Windows Brokering File System use-after-free LPE</span>
    <span class="vuln-writeup"></span>
  </article>

  <article class="vuln-entry">
    <a class="vuln-cve" href="https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50466">CVE-2026-50466</a>
    <span class="vuln-summary">Windows Brokering File System use-after-free LPE</span>
    <span class="vuln-writeup"></span>
  </article>

  <article class="vuln-entry">
    <a class="vuln-cve" href="https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50469">CVE-2026-50469</a>
    <span class="vuln-summary">Projected File System privileged file delete</span>
    <a class="vuln-writeup" href="{% post_url 2026-07-28-projected-file-system-file-delete-cve-2026-50469 %}">Writeup</a>
  </article>

  <article class="vuln-entry">
    <a class="vuln-cve" href="https://msrc.microsoft.com/update-guide/vulnerability/CVE-2026-50502">CVE-2026-50502</a>
    <span class="vuln-summary">Windows Event Logging Service remote code execution</span>
    <span class="vuln-writeup"></span>
  </article>
</div>
