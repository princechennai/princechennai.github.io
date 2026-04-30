---
layout: default
title: Prince Praveen | DevOps Engineer
---

<style>
.hero {
  text-align: center;
  padding: 80px 20px 60px;
}
.hero h1 {
  font-size: 2.8em;
  margin-bottom: 10px;
}
.hero p {
  font-size: 1.2em;
  color: #666;
}
.hero-buttons a {
  display: inline-block;
  margin: 10px;
  padding: 12px 22px;
  border-radius: 6px;
  text-decoration: none;
  font-weight: 600;
}
.btn-primary {
  background: #0d6efd;
  color: white;
}
.btn-secondary {
  border: 1px solid #ccc;
  color: #333;
}
.section {
  padding: 50px 20px;
  max-width: 900px;
  margin: auto;
}
.section h2 {
  border-bottom: 2px solid #eee;
  padding-bottom: 10px;
}
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
}
.card {
  padding: 20px;
  border: 1px solid #eee;
  border-radius: 10px;
  background: #fafafa;
}
.footer {
  text-align: center;
  padding: 40px;
  color: #777;
  font-size: 0.9em;
}
</style>

<div class="hero">

<h1>👋 Hi, I'm Prince Praveen</h1>
<h3>DevOps Engineer | SRE | Platform Builder</h3>

<p>
I build scalable, reliable cloud systems with a strong focus on automation, observability, and developer experience.
</p>

<div class="hero-buttons">
  <a href="./_posts/infrastructure-as-code-patterns.md" class="btn-primary">📖 View My Work</a>
  <a href="https://github.com/princechennai" target="_blank" class="btn-secondary">💻 GitHub</a>
  <a href="https://linkedin.com/in/princecrown" target="_blank" class="btn-secondary">🔗 LinkedIn</a>
  <a href="https://www.princepraveen.co.in" target="_blank" class="btn-secondary">💼 Portfolio</a>
</div>

</div>

---

<div class="section">

<h2>🚀 What I Do</h2>

<div class="grid">

<div class="card">
<h3>⚙️ Infrastructure as Code</h3>
<p>Designing reusable Terraform modules, multi-environment deployments, and governance-driven infrastructure.</p>
</div>

<div class="card">
<h3>☸️ Kubernetes & Containers</h3>
<p>Building scalable workloads on EKS/ECS with strong operational patterns and reliability.</p>
</div>

<div class="card">
<h3>🔁 CI/CD & Platform Engineering</h3>
<p>Creating self-service developer platforms using GitHub Actions and GitOps workflows.</p>
</div>

<div class="card">
<h3>📊 Observability & SRE</h3>
<p>Defining SLIs/SLOs, improving system reliability, and reducing operational toil.</p>
</div>

</div>

</div>

---

<!-- <div class="section">

<h2>📂 Featured Work</h2>

<div class="grid">

<div class="card">
<h3>🧩 Infrastructure as Code Patterns</h3>
<p>Enterprise Terraform patterns: modules, environments, and scaling strategies.</p>
<p><a href="./_posts/infrastructure-as-code-patterns.md">👉 Read Article</a></p>
</div>

<div class="card">
<h3>📊 SRE Metrics That Matter</h3>
<p>Understanding SLIs, SLOs, and how to measure reliability effectively.</p>
<p><a href="./_posts/sre-metrics-that-matter.md">👉 Read Article</a></p>
</div>

<div class="card">
<h3>🏗️ Multi-Tenant ECS Architecture</h3>
<p>Designing resilient multi-tenant systems with AWS ECS.</p>
<p><a href="./_posts/designing-resilient-multi-tenant-app-ecs.md">👉 Read Article</a></p>
</div>

</div>

</div> -->

---

<div class="section">

<h2>✍️ Latest Posts</h2>

<ul>
{% for post in site.posts limit:5 %}
  <li>
    <a href="{{ post.url }}">{{ post.title }}</a>
    <small> — {{ post.date | date: "%b %d, %Y" }}</small>
  </li>
{% endfor %}
</ul>

</div>

---

<div class="section">

<h2>📈 Philosophy</h2>

<ul>
<li>Build platforms, not just pipelines</li>
<li>Automate everything repeatable</li>
<li>Design for failure from day one</li>
<li>Make systems observable and predictable</li>
</ul>

</div>

---

<div class="section" style="text-align:center;">

<h2>📫 Let's Connect</h2>

<p>
<a href="https://github.com/princechennai" target="_blank" class="btn-primary">GitHub</a>
<a href="https://linkedin.com/in/princecrown" target="_blank" class="btn-secondary">LinkedIn</a>
</p>

</div>

---
