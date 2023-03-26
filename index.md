## BB Personal Website - Under construction

<!--
**JoseAAManzano/joseaamanzano** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

- 
- 
- 
- 
- 
- 

<!--
## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/JoseAAManzano/joseaamanzano.github.io/edit/main/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/JoseAAManzano/joseaamanzano.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.


### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
-->


{% include head.html %}

<body>
  <h1>Publications</h1>
  <ul id="pubs"></ul>

  <h1>ORCID Profile</h1>
  <ul>
    <li><strong>ORCID ID:</strong> <span id="orcid-id"></span></li>
    <li><strong>Name:</strong> <span id="name"></span></li>
    <li><strong>Email:</strong> <span id="email"></span></li>
    <li><strong>Employment:</strong> <span id="employment"></span></li>
    <li><strong>Education:</strong> <span id="education"></span></li>
  </ul>

  {% include footer.html %}

  <script>
    // Set your ORCID ID
    var orcid_id = "0000-0001-5533-2086";

    // Use the ORCID public API to retrieve your profile data
    fetch("https://pub.orcid.org/v3.0/" + orcid_id, {
      headers: {
        "Accept": "application/vnd.orcid+json"
      }
    }).then(function(response) {
      return response.json();
    }).then(function(data) {
      // Display your ORCID profile data on the web page
      document.getElementById("orcid-id").textContent = orcid_id;
      document.getElementById("name").textContent = data['person']['name']['given-names']['value'] + " " + data['person']['name']['family-name']['value'];
      document.getElementById("email").textContent = data['person']['emails']['email'][0]['email'];
      document.getElementById("employment").textContent = data['activities-summary']['employments']['employment-summary'][0]['organization']['name'];
      document.getElementById("education").textContent = data['activities-summary']['educations']['education-summary'][0]['organization']['name'];
    }).catch(function(error) {
      console.error(error);
    });

    // Use the ORCID public API to retrieve your publication data
    fetch("https://pub.orcid.org/v3.0/" + orcid_id + "/works", {
      headers: {
        "Accept": "application/vnd.orcid+json"
      }
    }).then(function(response) {
      return response.json();
    }).then(function(data) {
      // Retrieve the list of publications from the response data
      var publications = data['group'];

      // Loop through the publications and add them to an unordered list on the web page
      for (var i = 0; i < publications.length; i++) {
        var pub = publications[i]['work-summary'][0];
        // Only include publications that have been cited
        if (pub['cited-by-count'] > 0) {
          var li = document.createElement("li");
          var a = document.createElement("a");
          var title = pub['title']['title']['value'];
          var url = pub['url']['value'];
          a.textContent = title;
          a.href = url;
          li.appendChild(a);
          document.getElementById("pubs").appendChild(li);
        }
      }
    }).catch(function(error) {
      console.error(error);
    });
  </script>
</body>
