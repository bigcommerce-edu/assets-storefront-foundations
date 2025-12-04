# Catalyst Structure and Conventions

## React

**Example component:**

```javascript
function BlogPost({
  article,
  showTags
}) {
  {/* State to track a user's interaction */}
  const [liked, setLiked] = useState(false);

  const activateLike = () => {
    // ... send request
    setLiked(true);
  }

  return (
    <article>
      {/* Outputting a simple object value */}
      <h1>{article.title}</h1>

      <div>
        {article.body}
      </div>

      {/* Using comparison for an "IF" */}
      {showTags && (article.tags.length > 0) && (
        <ul>
          {/* A simple array function to loop over a value */}
          {article.tags.map(tag => (
            <li>{tag}</li>
          ))}
        </ul>
      )}

      <div>
        {/* Ternary operators work as well
        This time examining state for what to output. */}
        {liked ? (
          <span>You liked this</span>
        ) : (
          <button onClick={activateLike}>Like</button>
        )}
      </div>
    </article>
  );
}
```
