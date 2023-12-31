
# SQL query 

##  Product Title
```sql
SELECT 
  P.Title,
FROM 
  Product P
```

## Add Image Url for the images with the ImageIndex = 0. 

```sql
SELECT 
  P.Title,
  I.Url,
FROM 
  Product P
LEFT JOIN ProductImages PI ON P.Id = PI.ProductId AND PI.ImageIndex = 0
LEFT JOIN Image I ON PI.ImageId = I.Id
```

This type of query would make the database schema a candidate for denormalization because in case of using the Url of the primary image very often, one could add a PrimaryImageUrl column directly to the Product table, and update it by triggers or application logic each time the primary image changes.

 Always subjecting the decision to implement this functionality to simplicity and if similar cases occur in other tables of the database and the queries become very expensive.

## Product desc TranslatedText  or originalText in US code

```sql
SELECT 
  P.Title,
  COALESCE(PD.TranslatedText, PD.OriginalText) AS DescriptionText
FROM 
  Product P
LEFT JOIN ProductDescription PD ON P.Id = PD.ProductId AND PD.CountryCode = 'us';
``````

I had to look for the function that would be used in baseline SQL for this operation, my idea was to use IFNULL but I have seen that this functionality is from MYSQL and the backbone of the exercise would be PostgreSLQ so COALESCE seems to be the best option in this case.

## Desired query with all the data required

```sql
SELECT 
  P.Title,
  I.Url,
  COALESCE(PD.TranslatedText, PD.OriginalText) AS DescriptionText
FROM 
  Product P
LEFT JOIN ProductImages PI ON P.Id = PI.ProductId AND PI.ImageIndex = 0
LEFT JOIN Image I ON PI.ImageId = I.Id
LEFT JOIN ProductDescription PD ON P.Id = PD.ProductId AND PD.CountryCode = 'us';
``````
