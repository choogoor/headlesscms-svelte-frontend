<script>
  export let nodes;

  function serialize(children) {
    if (!children || children.length === 0) {
      return '';
    }

    return children.map((node) => {
      if (node.text) {
        let text = node.text;

        if (node.bold) {
          text = `<strong>${text}</strong>`;
        }

        if (node.code) {
          text = `<code>${text}</code>`;
        }

        if (node.italic) {
          text = `<em>${text}</em>`;
        }

        console.log(text);

        if (text.trim() === '') {
          return null;
        }

        return text;
      }

      if (!node) {
        return null;
      }

      switch (node.type) {
        case 'h1':
          return `<h1>${serialize(node.children)}</h1>`;
        case 'h6':
          return `<h6>${serialize(node.children)}</h6>`;
        case 'blockquote':
          return `<blockquote>${serialize(node.children)}</blockquote>`;
        case 'ul':
          return `<ul>${serialize(node.children)}</ul>`;
        case 'ol':
          return `<ol>${serialize(node.children)}</ol>`;
        case 'li':
          return `<li>${serialize(node.children)}</li>`;
        case 'link':
          return `<a href="${node.url}">${serialize(node.children)}</a>`;

        default:
          return `<p>${serialize(node.children)}</p>`;
      }
    }).filter(Boolean)
    .join('');;
  }
</script>

<div>{@html serialize(nodes)}</div>

<style>
  /* Add your styles here */
</style>
