# Extending Existing Resource

This example is based on the following document
https://docs.abp.io/en/abp/latest/Localization#extending-existing-resource

We will change the default `DisplayName:Abp.Timing.Timezone` and `Description:Abp.Timing.Timezone` of `AbpTimingResource` and add localized text in Russian language(`ru`).

I created the `AbpTiming` folder in the `Localization` directory of the `ExtendLocalizationResource.Domain.Shared` project.

Create en.json and ru.json in its directory.

`en.json`
```json
{
  "culture": "en",
  "texts": {
    "DisplayName:Abp.Timing.Timezone": "My Time zone",
    "Description:Abp.Timing.Timezone": "My Application time zone"
  }
}
```

`ru.json`
```json
{
  "culture": "ru",
  "texts": {
    "DisplayName:Abp.Timing.Timezone": "Часовой пояс",
    "Description:Abp.Timing.Timezone": "Часовой пояс приложения"
  }
}
```

Change the code of the `ConfigureServices` method in `ExtendLocalizationResourceDomainSharedModule`.

```cs
Configure<AbpLocalizationOptions>(options =>
{
    options.Resources
        .Add<ExtendLocalizationResourceResource>("en")
        .AddBaseTypes(typeof(AbpValidationResource))
        .AddVirtualJson("/Localization/ExtendLocalizationResource");

    //add following code
    options.Resources
        .Get<AbpTimingResource>()
        .AddVirtualJson("/Localization/AbpTiming");

    options.DefaultResourceType = typeof(ExtendLocalizationResourceResource);
});
```

Execute `ExtendLocalizationResource.DbMigrator` to migrate the database and run `ExtendLocalizationResource.Web`.

We have changed the English localization text and added Russian localization.

```CS
<p>@AbpTimingResource["DisplayName:Abp.Timing.Timezone"]</p>
@using(CultureHelper.Use("ru"))
{
    <p>@AbpTimingResource["DisplayName:Abp.Timing.Timezone"]</p>
}
```

![](data:image/png;base64,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)