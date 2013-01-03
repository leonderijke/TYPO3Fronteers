---
layout: post
title: Using additionalAttributes to add extra attributes to HTML form elements in TYPO3 Fluid
---

Imagine you're building a form with TYPO3 Fluid and want to add an HTML5 `placeholder` attribute to a textarea. Or you want to add some custom `data-*` attributes to some HTML element. In my case, I was designing the backend module for an imagegallery extension and wanted to use a `placeholder` attribute on a textarea.

My first guess was, based on my experience with `<f:form.textfield placeholder="Foo"/>`:

	<f:form.textarea rows="5" cols="20" placeholder="Foo"/>

But, alas, that threw an error: <q>Argument "placeholder" was not registered.</q>. I opened TextareaViewHelper.php to discover that, unlike the Textfield ViewHelper, there was indeed no `placeholder` attribute. (The TextareaViewHelper.php file is located in `typo3_src/typo3/sysext/fluid/Classes/ViewHelpers/Form/`, in the ViewHelpers folder you'll find all Fluid ViewHelpers.)

In the bugtracker I found <a href="http://forge.typo3.org/issues/27273" target="_blank" title="TYPO3 Bug #27273">a related bug</a> that was rejected. In the rejection comment an interesting clue was given: "For other (custom) attributes you can use the `additionalAttributes` argument which is supported by all tag based ViewHelpers".

And indeed, there's an `additionalAttributes` argument available on all tag based ViewHelpers that will take an array of attributes and adds them directly to the resulting HTML tag:

	<f:form.textarea name="mytextarea" rows="5" cols="20" additionalAttributes="{placeholder: 'Foo', data-bar: 'baz'}"/>

will output:

	<textarea placeholder="Foo" data-bar="baz" rows="5" cols="20" name="mytextarea"></textarea>

On a side note: in <a href="http://forge.typo3.org/issues/43081" target="_blank" title="TYPO3 Bug #43081">bug #43081</a> all ViewHelpers will be changed to a default HTML5 rendering, since the TYPO3 default rendering is HTML5. So, the placeholder attribute will probably be soon available on the Textarea ViewHelper too.