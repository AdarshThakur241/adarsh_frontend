Hosted Link: https://6443f316dac83e4111cc9425--fabulous-macaron-caf79d.netlify.app/
Pdf file link:- https://docs.google.com/document/d/1Rdy6vp4h3up5VxsXxwOMK36xgIGfhDtR/edit?usp=share_link&ouid=115233992373451957387&rtpof=true&sd=true
Name: Adarsh Onkar Thakur
Registration Number: 12011162

1.Explain what the simple List component does?

The List component is basically rendering the list of items, in which each items gets selected as soon as you click on it. The background colour changes depending on the condition whether it is selected or not. The component takes a single prop, items, which is an array of objects. When an item is clicked, its background color changes to green to indicate that it is selected. If another item is clicked, the previously selected item's background color changes back to red, and the new item's background color changes to green. The memo is used to enhance the performance of the site. Prop-types is used to validate the data-types of the data passed and as soon as the wrong data type is passed it throws error.

2. What problems/warning  are there with the code?

1. Uncaught TypeError: PropTypes.shapeOf is not a function, the propTypes declaration for the items prop in the WrappedListComponent component is wrong.

Error in the code :

WrappedListComponent.propTypes = {
  items: PropTypes.array(PropTypes.shapeOf({ // Function is not valid
    text: PropTypes.string.isRequired,
  })),
 };


2. The onClickHandler property in WrappedSingleList Item is not called correctly it should be called in callback

Error in the code:

const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={onClickHandler(index)}>   //callback is not defined 
      {text}
    </li>
  );
};

3. The value is null for the items prop in WrappedListComponent .

Error in the code:

WrappedListComponent.defaultProps = {
  items: null,
};

4. To make sure that selected item changes the colour not all the items

Error in the code:

return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))} </ul>
  )};

5. Uncaught TypeError: setSelectedIndex is not a function in the code given. In the useState, but the initial value (null) should appear first in the returned array, not the setter function.

Error in the code:

 const [setSelectedIndex, selectedIndex] = useState(); //incorrect sequence






6. The value of the unique key is not defined as a prop for each child value.

Error in the Code:

return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex=== index}
        />
      ))}
    </ul>
  )
};

3. Please fix, optimize, and/or modify the component as much as you think is necessary.

import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={() => onClickHandler(index)} //modified 
      > 
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({
  items,
}) => {
  const [selectedIndex, setSelectedIndex] = useState(); //Modified

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(index);
  };

  return (

    < ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index} //modified
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index} //modified
        />
      ))}

    </ul>
  )
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({ //Modified
    text: PropTypes.string.isRequired,
  })),
};

WrappedListComponent.defaultProps = { //Modified
  items: [
    { text: "Adarsh" },
    { text: "Shivam" },
    { text: "Prince" },
    { text: "Aayush" },
    ],
};
const List = memo(WrappedListComponent);

export default List;






