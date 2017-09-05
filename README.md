# FME® ListAppender

##About
The _ListAppender_ adds new elements to an existing list, starting at a desired key or at the first available one. The user can set the values for the elements, based on constants, parameters or other feature attributes.

###IMPORTANT
A user can specify a regular attribute for the _List Attribute_ input parameter of this transformer, which will result in _all features being rejected_. The reason why this is possible in the first place, is because the _ListAppender_ also supports nested lists, but FME has no parameter type that only allows selection of the "deepest nested list". Therefore, the only way this can be achieved, is by using a regular attribute input parameter, that will show the list key selection dialog once a list attribute has been selected. Below are a couple of examples that explain how the _List Attribute_ parameter can be used.

**Example 1**  
If the user wants to append elements at the end of nested list _parent{3}.child{}_ (i.e. append children to parent nr. 3), the _List Attribute_ input parameter should be set to _parent{**3**}.child{**0**}_. As a general rule, setting the last key to 0 means "just append the values at the end of whatever is there already". This means that even if _parent{3}.child{}_ contains 6 elements (keys 0 - 5) already, the _ListAppender_ will start appending values at key 6. No elements will _ever_ be overwritten.

**Example 2**  
If the user wants to append elements starting at _exactly_ key 8 of list _example{}_ (i.e. add elements _example{8}_, _example{9}_, etc.), the _List Attribute_ input parameter should be set to _example{**8**}_. Note that if this list contains more than 8 elements already (e.g. 10 elements, so keys 0 - 9), the new ones will be appended starting at key 10 and further. No elements will _ever_ be overwritten.

**Example 3**  
If the user wants to append elements at the end of list _example{}.value_, the _List Attribute_ parameter could be set to _example{**0**}.value_. If the user has specified 3 new items to add and the list already contained 2 elements, the result will be:  
_example{0}.value = 'existing item1'_  
_example{1}.value = 'existing item2'_  
_example{2}.value = 'new item1'_  
_example{3}.value = 'new item2'_  
_example{4}.value = 'new item3'_  

Note that if the _ListAppender_ failed to append attributes to the list, the feature will be rejected and a \__listappender\_error_ attribute containing the failure reason will be added.

###Notes  
- Also available on [FME Hub](https://hub.safe.com/transformers/listappender) for convenient installation.
- This transformer has been tested on Python 2.7 and 3.4.  
- Released under [GNU General Public License v3.0](https://github.com/SanderSchaminee/fme-listappender/blob/master/LICENSE).  
- If you notice a bug or desire a new feature, please contact me. Or make a pull request!

##Usage

**List Attribute**  
Select the list to which you would like to add new elements. Note that you can select regular attributes as well, but this will not work. Please refer to the examples in the Overview section.

**Values to Add**  
Sets the element values you would like to append to the list. The number of values you specify here equals the number of elements that will be added. You can choose to insert values from other (list) attributes, parameters or constants, including _null_.
