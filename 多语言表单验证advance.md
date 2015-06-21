1、Adding dynamic field
```js
总结几点:
clone块的方法
$clone    = $template
                                .clone()
                                .removeClass('hide')
                                .removeAttr('id')
                                .insertBefore($template),
                                
同名时验证新增的filed
// Add new field
            $('#surveyForm').formValidation('addField', $option);
同名时删除的filed的删除验证
// Remove element containing the option
            $row.remove();

            // Remove field
            $('#surveyForm').formValidation('removeField', $option);
尤其注意不同名时，验证函数是独立放出来的，学习其调用方法。
不同名时验证新增的filed
 $('#bookForm')
                .formValidation('addField', 'book[' + bookIndex + '].title', titleValidators)
                .formValidation('addField', 'book[' + bookIndex + '].isbn', isbnValidators)
                .formValidation('addField', 'book[' + bookIndex + '].price', priceValidators);
同名时删除的filed的删除验证
  
            // Remove fields
            $('#bookForm')
                .formValidation('removeField', $row.find('[name="book[' + index + '].title"]'))
                .formValidation('removeField', $row.find('[name="book[' + index + '].isbn"]'))
                .formValidation('removeField', $row.find('[name="book[' + index + '].price"]'));

            // Remove element containing the fields
            $row.remove();
  可以在不存在这个属性的时候通过设置·属性来新增属性
   .attr('data-book-index', bookIndex)
```
