## updating repos.json

```js
await (async () => {
  const groupId = 'clo';
  let allProjects = [];
  let page = 1;
  let hasNextPage = true;

  while (hasNextPage) {
    const response = await fetch(`/api/v4/groups/${groupId}/projects?include_subgroups=true&per_page=100&page=${page}`);
    const projects = await response.json();

    if (projects.length > 0) {
      allProjects = allProjects.concat(projects);
      console.log(`page ${page}: ${projects.length} repos (total: ${allProjects.length})`);
      page++;
    } else {
      hasNextPage = false;
    }

    await new Promise(resolve => setTimeout(resolve, 300));
  }

  const result = allProjects.map(p => ({
    name: p.path_with_namespace.slice(4),
    clone_url: p.http_url_to_repo,
    description: p.description
  }));

  console.log(JSON.stringify(
    result.sort((a, b) => a.name < b.name ? -1 : a.name > b.name ? 1 : 0),
    null, 2
  ));
})();
```

on https://git.codelinaro.org
