# Workflow-Studio-Embedding

Published WASM project for hosting in Angular/other frameworks.

## Overview

This repository contains the published files from the [Elsa Studio Blazor WASM guide](https://github.com/elsa-workflows/elsa-guides/tree/main/src/installation/elsa-studio/ElsaStudioBlazorWasm).

**Based on Version:** 3.5.3

## How to Re-create the Project

1. Pull the [source repository](https://github.com/elsa-workflows/elsa-guides/tree/main/src/installation/elsa-studio/ElsaStudioBlazorWasm)
2. Build the project
3. Right-click the Web Project and publish to a folder

## Customization Files

This project includes additional customization files:

### 1. Custom Theme CSS

Removes unnecessary buttons and aligns colors with Magnar themes.

```html
<link href="custom-theme.css" rel="stylesheet">
```

### 2. Navigation Bar Script

Re-creates the nav-bar buttons as a header navigation instead of a side panel.

```html
<script>
(function() {
    function injectTopNav() {
        if (document.getElementById('magnar-top-nav')) return true;
        
        const appContainer = document.getElementById('app');
        if (!appContainer) return false;
        
        const nav = document.createElement('div');
        nav.id = 'magnar-top-nav';
        nav.classList = ['marg-top-start-section'];
        
        const defBtn = document.createElement('a');
        defBtn.href = 'workflows/definitions';
        defBtn.textContent = 'Definitions';
        defBtn.className = 'magnar-nav-btn';
        
        const instBtn = document.createElement('a');
        instBtn.href = 'workflows/instances';
        instBtn.textContent = 'Instances';
        instBtn.className = 'magnar-nav-btn';
        
        nav.appendChild(defBtn);
        nav.appendChild(instBtn);
        
        document.body.insertBefore(nav, document.body.firstChild);
        updateActiveState();
        
        return true;
    }
    
    function updateActiveState() {
        const currentPath = window.location.pathname;
        const buttons = document.querySelectorAll('.magnar-nav-btn');
        buttons.forEach(btn => {
            const href = btn.getAttribute('href');
            btn.classList.toggle('active', currentPath.includes(href));
        });
    }
    
    function setupObserver() {
        const observer = new MutationObserver(function() {
            if (!document.getElementById('magnar-top-nav')) {
                injectTopNav();
            }
            updateActiveState();
        });
        
        const appContainer = document.getElementById('app');
        if (appContainer) {
            observer.observe(appContainer, { childList: true, subtree: true });
        }
    }
    
    function init() {
        if (injectTopNav()) {
            setupObserver();
        } else {
            setTimeout(init, 500);
        }
    }
    
    if (document.readyState === 'loading') {
        document.addEventListener('DOMContentLoaded', () => setTimeout(init, 1000));
    } else {
        setTimeout(init, 1000);
    }
    
    window.addEventListener('load', () => setTimeout(init, 1500));
})();
</script>
```
