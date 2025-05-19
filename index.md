---
layout: default
title: AI Prompt Templates
---

<div class="container">
  <h1>AI Prompt Templates</h1>
  
  <div class="form-container">
    <div class="form-group">
      <label for="category">Category:</label>
      <select id="category" class="form-control">
        <option value="">Select a category</option>
        {% for category in site.data.templates.categories %}
          <option value="{{ category.name }}">{{ category.name }}</option>
        {% endfor %}
      </select>
    </div>

    <div class="form-group">
      <label for="template">Template:</label>
      <select id="template" class="form-control" disabled>
        <option value="">Select a template</option>
      </select>
    </div>
  </div>

  <div id="template-content" class="template-content" style="display: none;">
    <div class="template-header">
      <h2 id="template-title"></h2>
      <button id="copy-button" class="copy-button">Copy to Clipboard</button>
    </div>
    <pre id="template-text" class="template-text"></pre>
  </div>
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  const categorySelect = document.getElementById('category');
  const templateSelect = document.getElementById('template');
  const templateContent = document.getElementById('template-content');
  const templateTitle = document.getElementById('template-title');
  const templateText = document.getElementById('template-text');
  const copyButton = document.getElementById('copy-button');

  // Store templates data
  const templates = {{ site.data.templates.categories | jsonify }};

  // Handle category selection
  categorySelect.addEventListener('change', function() {
    const selectedCategory = this.value;
    templateSelect.innerHTML = '<option value="">Select a template</option>';
    templateSelect.disabled = !selectedCategory;
    templateContent.style.display = 'none';

    if (selectedCategory) {
      const category = templates.find(cat => cat.name === selectedCategory);
      if (category) {
        category.templates.forEach(template => {
          const option = document.createElement('option');
          option.value = template.name;
          option.textContent = template.name;
          templateSelect.appendChild(option);
        });
      }
    }
  });

  // Handle template selection
  templateSelect.addEventListener('change', function() {
    const selectedCategory = categorySelect.value;
    const selectedTemplate = this.value;
    
    if (selectedCategory && selectedTemplate) {
      const category = templates.find(cat => cat.name === selectedCategory);
      const template = category.templates.find(t => t.name === selectedTemplate);
      
      if (template) {
        templateTitle.textContent = template.name;
        templateText.textContent = template.content;
        templateContent.style.display = 'block';
      }
    } else {
      templateContent.style.display = 'none';
    }
  });

  // Handle copy button
  copyButton.addEventListener('click', function() {
    const text = templateText.textContent;
    navigator.clipboard.writeText(text).then(() => {
      const originalText = this.textContent;
      this.textContent = 'Copied!';
      setTimeout(() => {
        this.textContent = originalText;
      }, 2000);
    });
  });
});
</script>

<style>
.container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
}

.form-container {
  margin-bottom: 30px;
}

.form-group {
  margin-bottom: 20px;
}

.form-control {
  width: 100%;
  padding: 8px;
  font-size: 16px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.template-content {
  background: #f8f9fa;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 20px;
  margin-top: 20px;
}

.template-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
}

.template-text {
  background: white;
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 4px;
  white-space: pre-wrap;
  font-family: monospace;
  margin: 0;
}

.copy-button {
  background: #007bff;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.copy-button:hover {
  background: #0056b3;
}
</style> 