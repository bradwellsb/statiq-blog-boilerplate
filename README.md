# Blog Boilerplate for [Statiq.Web](https://github.com/statiqdev/Statiq.Web)
This boilerplate is designed to help you quickly get your statically generated blog up and running using Statiq.Web. It includes an RSS feed, a customizable sidebar, and widgets such as categories and date archives. Please respect the [licensing terms for Statiq.Web](https://github.com/statiqdev/Statiq.Web/blob/master/LICENSE-FAQ.md).

![](https://wellsb.com/csharp/wp-content/uploads/sites/2/2020/05/statiq-blog-boilerplate2-min.png)

## How to use the boilerplate
Making customizations to your blog using the boilerplate is easy. Here is a detailed [write-up on the subject](https://wellsb.com/csharp/aspnet/create-blog-csharp-statiq/). Following are some common configuration suggestions.

- To add your own blog posts, create Markdown (.md) files, or use any other supported syntax and place them in the **/input/blog/posts/** directory. There are sample posts included in the boilerplate.

- To change your website title, blog title, and other details, edit the appropriate value in **Constants.cs**.

- To change the subdirectory for your blog from **/blog/** to another path, change the name of the **blog** folder in the **input** directory and set the **BlogPath** string in **Constants.cs** to the new location.

- To change your permalink structure, change line 24 of **Program.cs**. By default, the boilerplate template uses `/{year}/{category}/{title}` for generating permalinks.

- The sidebar is displayed by default. To hide it, add `HideSidebar: true` markup to the pages where you do not wish to show the sidebar.

- To change the order of items in the sidebar, or to add your own widgets, simply edit **/input/blog/\_sidebar.cshtml**. If you want the sidebar to display on the left side of the page. Simply move things around in **/input\/_layout.cshtml**.

- To adjust how posts are displayed in index and archive pages, adjust **/input/blog/\_posts.cshtml**. For example, you could create a card-style display by using the following code:

```html
<div class="row">
    @foreach (IDocument post in Model)
    {
        IDocument topicDocument = Outputs[nameof(Archives)][$"{Constants.BlogPath}/categories/{post.GetString("Category")}/index.html"];
        string excerpt = post.GetString(Statiq.Html.HtmlKeys.Excerpt);
        excerpt = excerpt.RemoveEnd("");
        var words = excerpt.Split().Take(50);
        excerpt = string.Join(" ", words);
        excerpt += $" ... <span class=\"small\"><a href=\"{Context.GetLink(post)}\">Read more</a></span></p>";
 
        <div class="col-12 col-md-6 @(Document.GetBool("HideSidebar")?"col-lg-4 col-xl-3":"col-xl-4") mb-3">
            <div class="card">
                <div class="card-header">
                    <h2 itemprop="headline name" class="card-title">
                        <a itemprop="url" class="small" href="@Context.GetLink(post).Substring(1)">@post.GetString("Title")</a>
                    </h2>
                    <div class="bh">
                        <time datetime="@post.GetString("Published")" itemprop="datePublished">@post.GetDateTime("Published").ToString("MMMM dd, yyyy")</time>
                        @if (topicDocument != null)
                        {
                            <a href="@(topicDocument.GetLink())"><span class="badge badge-info">@topicDocument.GetTitle()</span></a>
                        }
                    </div>
                </div>
                <div class="card-body">
                    <div itemprop="articleBody">
                        <div class="card-text">@Html.Raw(excerpt)</div>
                    </div>
                </div>
            </div>
            <div class="collapse" aria-hidden="true" itemprop="author" itemscope="" itemtype="http://schema.org/Person">
                <meta itemprop="name" content="@post.GetString("Author")">
            </div>
            <meta itemprop="dateModified" content="@post.GetString("Modified")">
        </div>
    }
</div>
```
![](https://wellsb.com/csharp/wp-content/uploads/sites/2/2020/05/statiq-blog-boilerplate-cards-min.png)

Thanks to the following projects who currently use [Statiq.Web](https://statiq.dev/web/). Their code was used as a reference for designing this boilerplate.
- [Net Foundation website](https://github.com/dotnet-foundation/website)
- [Statiq.dev website](https://github.com/statiqdev/statiqdev.github.io)
