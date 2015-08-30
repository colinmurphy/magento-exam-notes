### 1. Which method is used for translating strings, and on which types of objects is it generally available?


- Module Translation:	CSV in /app/locale
- Theme Translation:	CSV in /app/design/{area}/{package}/{theme{}/locale (theme folder translate)
- Database Translation:	Database (table core_translate)


### 2. In what way does the developer mode influence how Magento handles translations?

Not sure how to answer but making strings translatable.


### 3. How many options exist to add a custom translation for any given string?

- Database Translation
- Module Translation
- Theme Translation

### 4. What is the priority of translation options?

- Database Translations
- Theme Translations
- Module Translation


### 5. How are translation conflicts (when two modules translate the same string) processed by Magento?

**Example:**

    Mage::helper("module_a")->__("Hello");
    Mage::helper("module_b")->__("Hello");

In the method _addData() will create 2 keys for each module to keep module string separate.
