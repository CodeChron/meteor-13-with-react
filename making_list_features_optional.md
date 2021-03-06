_Get caught up to this step: Check out branch 06-delete-note - see Introduction for info on how to get caught up using branches._
<hr>
# Step 7: Making List Features Optional

Let's make the add item form and the delete button optional in the list.  This will make the List component more versatile and easier to re-use.

## Move optional features into a collection

``` /imports/components/lists/list.jsx ```

```js
...
	const optionalFeatures = {
  	addItem: () => <li className="list-group-item"><SingleFieldSubmit {...props} /></li>,
  	deleteItem: (args) => <span className="pull-right"><DeleteBtn itemToDelete={args} {...props}/></span>
	}
...
}
```

## Create a function for accessing a feature and passing in arguments

```js
...
	const displayOptionalFeature = (shouldDisplay, feature, args ) => {
	  return shouldDisplay? feature(args) : null
	}
...
}
```

## Use the function to dynamically display a feature

```js
...
	return <ul>
	    {displayOptionalFeature(props.addItem, optionalFeatures.addItem)}
	    { 
	    	props.collection.map((item) => {
	 	      return <li key={item._id}>{item.content} {displayOptionalFeature(props.deleteItem, optionalFeatures.deleteItem, item)}
	 	      </li>

	      })
	    }
  </ul>
}

```

## Add propTypes and defaultProps to hide features by default

```js
List.propTypes = {
	collection: React.PropTypes.array.isRequired,
	addItem: React.PropTypes.bool,
	deleteItem:  React.PropTypes.bool,
}
List.defaultProps = {
	addItem: false,
	deleteItem: false
}
```


## "Turn on" the features via props

``` /imports/containers/note_container.jsx ```

```js
  return {
     ...
	  addItem: true,
	  deleteItem: true
  }
```

Another advantage, in addition to component versatility, is feature scalability.  We can now easily add numerous optional features to this list.