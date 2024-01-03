# SilverStripe Excel Import Export module

![Build Status](https://github.com/lekoala/silverstripe-excel-import-export/actions/workflows/ci.yml/badge.svg)
[![scrutinizer](https://scrutinizer-ci.com/g/lekoala/silverstripe-excel-import-export/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/lekoala/silverstripe-excel-import-export/)
[![Code coverage](https://codecov.io/gh/lekoala/silverstripe-excel-import-export/branch/master/graph/badge.svg)](https://codecov.io/gh/lekoala/silverstripe-excel-import-export)

## Intro

Replace all csv import/export functionalities by Excel.
Excel support is provided by `spread-compat` package which can use under the hood simple xlsx, php spreadsheet or openspout.

These changes apply automatically to SecurityAdmin and ModelAdmin through extension.

To make import easier, import specs are replaced by a sample file that is ready to use for the user.
This import file can be further customised by implementing `sampleExcelFile` method.

## Configure exported fields

All fields are exported by default (not just summary fields that are useless by themselves)

If you want to restrict the fields, you can either:

- Implement a `exportedFields` method on your model that should return an array of fields
- Define a `exported_fields` config field on your model that will restrict the list to these fields
- Define a `unexported_fields` config field on your model that will blacklist these fields from being exported

## Custom import handlers

If you define a `listImportHandlers` you can define a list of custom handlers that your user can choose instead
of the default process.

These handler may or may not enable the `onlyUpdate` feature that will prevent creating new records. This
needs to be handled in your own import classes by adding a `setOnlyUpdate` method.

This require some custom code on your `ModelAdmin` class that could look like this

```php
   public function import($data, $form, $request)
    {
        if (!ExcelImportExport::checkImportForm($this)) {
            return false;
        }
        $handler = $data['ImportHandler'] ?? null;
        if ($handler == "default") {
            return parent::import($data, $form, $request);
        }
        return ExcelImportExport::useCustomHandler($handler, $form, $this);
    }
```

The import handlers only need to implement a `load` method that needs to return a result string
or a `BulkLoader_Result` object.

## Compatibility

Tested with ^5 and up

## Maintainer

LeKoala - thomas@lekoala.be
