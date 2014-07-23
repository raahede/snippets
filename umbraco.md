Umbraco
=======

## Navigation

### Partial
`Views/global/navigation.cshtml`
```cs
@inherits UmbracoTemplatePage

@* Helper method to travers through all descendants *@
@helper traverse( dynamic node, int rootLevel, int levels )
{
  // If a MaxLevel parameter is passed, otherwise default to 1 level
  int maxLevel = rootLevel + levels;

  // Select visible children
  var items = node.Children.Where("UmbracoNaviHide == false").Where("Level <=" + maxLevel);
  // Level in relation to root level
  int level = node.Level - rootLevel + 1;

  // If any items are returned, render a list
  if (items.Any())
  {
    <ul class="nav__list@(level > 1 ? "--level" + level : null)">
      @foreach (var item in items)
      {
        if( item.GetTemplateAlias() != "" ) {
          var url = ( item.DocumentTypeAlias == "HomePage" ) ? "/" : item.Url;
          bool inPath = CurrentPage.IsDescendantOrSelf( item );
          // If the Id of the item is the same as the Id of the current page then we want to add the class "current"
          // Otherwise, we set the class to null, that way it will not even be added to the <li> element
          <li class="nav__item@(CurrentPage.Id == item.Id ? " current" : null)@(inPath ? " active" : null)">
            <a class="nav__link" href="@url" title="@item.Name">@item.Name</a>
            @if( inPath ) {
              // Run the traverse helper again
              @traverse( item, rootLevel, levels );
            }
          </li>
        }
      }
    </ul>
  }
}

@{
  // Model.Content is the current page that we're on
  var rootId = ViewData["startNodeId"];
  var root = Umbraco.Content( rootId );
  int levels = ViewData.ContainsKey("levels") ? int.Parse( ViewData["levels"].ToString() ) : 1;
}

@* Render the navigation by passing the root node to the traverse helper *@
@traverse( root, root.Level, levels )
```

### Use

```cs
@Html.Partial( "global/navigation"
  , Model
  , new ViewDataDictionary {{ "startNodeId", Model.Id }, { "levels", 3 }})
```

### Output
``` html
<ul class="nav__list">
  <li class="nav__item active">
    <a class="nav__link" href="..." title="...">Level 1</a>
      <ul class="nav__list--level2">
      <li class="nav__item current active">
        <a class="nav__link" href="..." title="...">Level 2</a>
        <ul class="nav__list--level3">
          <li class="nav__item">
            <a class="nav__link" href="..." title="...">Level 3</a>
          </li>
        </ul>
      </li>
      <li class="nav__item">
        <a class="nav__link" href="..." title="...">Level 2</a>
      </li>
    </ul>
  </li>
</ul>
```
