# Target API reference \(new\)

## Get custom visitor IDs

Use this API to get the custom visitor ID for Target.

{% tabs %}
{% tab title="Android" %}
### getThirdPartyId

The callback is invoked to return the `thirdPartyId` value, or if no third-party ID is set, `null` is returned.

#### Syntax

```java
public static void getThirdPartyId(final AdobeCallback<String> callback)
```

#### Example

```java
Target.getThirdPartyId(new AdobeCallback<String>() {                    
    @Override
    public void call(String thirdPartyID) {
                // read target's thirdPartyID
    }
});
```
{% endtab %}

{% tab title="iOS" %}
### getThirdPartyId

Gets the custom visitor ID for Target. The callback will be invoked to return the `thirdPartyId` value, or if no third-party ID is set, `nil` is returned.

### Syntax   <a id="syntax"></a>

```objectivec
+ (void) getThirdPartyId: (nonnull void (^) (NSString* __nullable thirdPartyId)) callback;
```

### Examples   <a id="examples"></a>

Here are the examples in Objective C and Swift:

#### **Objective C**   <a id="objective-c"></a>

```objectivec
[ACPTarget getThirdPartyId:^(NSString *thirdPartyId){
       // read thirdPartyId
}];
```

#### **Swift**   <a id="swift"></a>

```swift
ACPTarget.getThirdPartyId({thirdPartyID in
       // read thirdPartyId
})
```
{% endtab %}
{% endtabs %}

## Set custom visitor IDs

Use this API to set custom visitor IDs for Target.

{% hint style="info" %}
This ID is preserved between app upgrades, is saved and restored during the standard application backup process, and is removed at uninstall or when the reset experience API is used.
{% endhint %}

{% tabs %}
{% tab title="Android" %}
### setThirdPartyId

#### Syntax

```java
public static void setThirdPartyId(final String thirdPartyId)
```

#### Example

```java
Target.setThirdPartyId("third-party-id");
```
{% endtab %}

{% tab title="iOS" %}
### setThirdPartyId

Sets the custom visitor ID for Target. This ID is preserved between app upgrades, is saved and restored during the standard application backup process, and is removed at uninstall or when `resetExperience` API is called.

### Syntax   <a id="syntax-1"></a>

```objectivec
+ (void) setThirdPartyId: (nullable NSString*) thirdPartyId;
```

### Examples   <a id="examples-1"></a>

Here are some examples in Objective-C and Swift:

#### **Objective-C**   <a id="objective-c-1"></a>

```objectivec
[ACPTarget setThirdPartyId:@"third-party-id"];
```

**Swift**

```swift
ACPTarget.setThirdPartyId("third-party-id")
```
{% endtab %}
{% endtabs %}

## Reset user experience

Use this API to reset the user's experience by removing the visitor identifiers. This method also removes previously set third-party and Target IDs from persistent storage.

{% tabs %}
{% tab title="Android" %}
### resetExperience

#### Syntax

```java
public static void resetExperience()
```

#### Example

```java
Target.resetExperience();
```
{% endtab %}

{% tab title="iOS" %}
### resetExperience

### Syntax   <a id="syntax-2"></a>

```objectivec
+ (void) resetExperience;
```

### Examples   <a id="examples-2"></a>

Here are some examples in Objective-C and Swift:

#### **Objective-C**   <a id="objective-c-2"></a>

```objectivec
[ACPTarget resetExperience];
```

#### **Swift**   <a id="swift-1"></a>

```swift
ACPTarget.resetExperience()
```
{% endtab %}
{% endtabs %}

## Get Target user identifier

Gets the Target user identifier.

{% tabs %}
{% tab title="Android" %}
### getTntId

The callback is invoked to return the `tntId` value 0 if no Target ID is set, and `null` is returned.

Target returns `tntId` with a successful call to `loadRequests` or `prefetchContent`. Once set in the SDK, this ID is preserved between app upgrades, is saved and restored during the standard application backup process, and is removed at uninstall or when `resetExperience` API is called.

#### Syntax

```java
public static void getTntId(final AdobeCallback<String> callback)
```

#### Example

```java
Target.getTntId(new AdobeCallback<String>() {
    @Override
    public void call(String value) {
        // read target's tntid
    }
});
```
{% endtab %}

{% tab title="iOS" %}
### getTntId

Gets the Target user identifier. The callback will be invoked to return the `tntId` value, or if no Target ID is set, `nil` is returned.

Target returns `tntId` upon a successful call to `loadRequests` or `prefetchContent`. Once set in SDK, this ID is preserved between app upgrades, is saved and restored during the standard application backup process, and is removed at uninstall or when `resetExperience` API is called.

#### Syntax

```objectivec
+ (void) getTntId: (nonnull void (^) (NSString* __nullable tntId)) callback;
```

#### **Examples**

**Objective-C**

```objectivec
[ACPTarget getTntId:^(NSString *tntId){
       // read target's tntId
}];
```

**Swift**

```swift
ACPTarget.getTntId({tntId in
       // read target's tntId
})
```
{% endtab %}
{% endtabs %}

## Retrieve Location Content requests

Executes a batch request to the configured Target server for multiple mbox locations. The main difference with `loadRequests` API is in usage along with prefetch APIs. This API only returns the content and does not increase the reporting count. If you are using normal batch requests, then there is no difference with `loadRequests` API.

{% tabs %}
{% tab title="Android" %}
### retrieveLocationContent

Sends a batch request to your configured Target server for multiple mbox locations that are specified in the `TargetRequest` list. Any prefetched content which matches a given mbox location is returned and not included in the batch request to the Target server. Each object in the list contains a callback function, which will be invoked when content is available for its given mbox location.

#### Syntax

```java
public static void retrieveLocationContent(final List<TargetRequest> targetRequestList,
                                           final TargetParameters parameters)
```

#### Example

```java
// define parameters for first request
Map<String, Object> mboxParameters1 = new HashMap<>();
mboxParameters1.put("status", "platinum");

// define parameters for second request
Map<String, Object> mboxParameters2 = new HashMap<>();
mboxParameters2.put("userType", "paid");

List<String> purchasedIds = new ArrayList<String>();
purchasedIds.add("34");
purchasedIds.add("125");

TargetOrder targetOrder = new TargetOrder("ADCKKIM", 344.30, purchasedIds);
TargetProduct targetProduct = new TargetProduct("24D3412", "Books");

TargetParameters parameters1 = new TargetParameters.Builder().parameters(mboxParameters1).build();
TargetRequest request1 = new TargetRequest("mboxName1", parameters1, "defaultContent1",
                                            new AdobeCallback<String>() {
                                                @Override
                                                public void call(String value) {
                                                    // do something with target content.
                                                }
                                            });

TargetParameters parameters2 = new TargetParameters.Builder()
                               .parameters(mboxParameters1)
                               .product(targetProduct)
                               .order(targetOrder)
                               .build();
TargetRequest request2 = new TargetRequest("mboxName2", parameters2, "defaultContent2",
                                            new AdobeCallback<String>() {
                                                @Override
                                                public void call(String value) {
                                                    // do something with target content.
                                                }
                                            });

// Creating Array of Request Objects
List<TargetRequest> locationRequests = new ArrayList<>();
locationRequests.add(request1);
locationRequests.add(request2);

 // Define the profile parameters map.
Map<String, Object> profileParameters1 = new HashMap<>();
profileParameters1.put("ageGroup", "20-32");

TargetParameters parameters = new TargetParameters.Builder().profileParameters(profileParameters1).build();
// Call the targetRetrieveLocationContent API.
Target.retrieveLocationContent(locationRequests, parameters);
```
{% endtab %}

{% tab title="iOS" %}
### retrieveLocationContent

Sends a batch request to your configured Target server for multiple mbox locations that are specified in the`ACPTargetRequestObject` array. Each object in the array contains a callback function, which will be invoked when content is available for its given mbox location.

#### Syntax

```objectivec
+ (void) retrieveLocationContent: (nonnull NSArray<ACPTargetRequestObject*>*) requests
                  withParameters: (nullable ACPTargetParameters*) parameters;
```

#### Example

```objectivec
NSDictionary *mboxParameters1 = @{@"status":@"platinum"};
ACPTargetProduct *product1 = [ACPTargetProduct targetProductWithId:@"24D3412" categoryId:@"Books"];
ACPTargetOrder *order1 = [ACPTargetOrder targetOrderWithId:@"ADCKKIM" total:@(344.30) purchasedProductIds:@[@"a", @"b"]];

NSDictionary *mboxParameters2 = @{@"userType":@"Paid"};
ACPTargetProduct *product2 = [ACPTargetProduct targetProductWithId:@"764334" categoryId:@"Online"];
NSArray *purchaseIDs = @[@"id1",@"id2"];
ACPTargetOrder *order2 = [ACPTargetOrder targetOrderWithId:@"4t4uxksa" total:@(54.90) purchasedProductIds:purchaseIDs];

ACPTargetParameters *params1 = [ACPTargetParameters targetParametersWithParameters:mboxParameters1
                                                    profileParameters:nil
                                                              product:product1
                                                                order:order1];
ACPTargetRequestObject *request1 = [ACPTargetRequestObject targetRequestObjectWithName:@"logo" targetParameters:params1
defaultContent:@"BlueWhale" callback:^(NSString * _Nullable content) {
    // do something with the received content
  }];

ACPTargetParameters *params2 = [ACPTargetParameters targetParametersWithParameters:mboxParameters2
                                                    profileParameters:nil
                                                              product:product2
                                                                order:order2];
ACPTargetRequestObject *request2 = [ACPTargetRequestObject targetRequestObjectWithName:@"logo" targetParameters:params2
defaultContent:@"red" callback:^(NSString * _Nullable content) {
    // do something with the received content
  }];
// create request object array
NSArray *requestArray = @[request1,request2];

// Creating Target parameters
NSDictionary *mboxParameters = @{@"status":@"progressive"};
NSDictionary *profileParameters = @{@"age":@"20-32"};
ACPTargetProduct *product = [ACPTargetProduct targetProductWithId:@"24D334" categoryId:@"Stationary"];
ACPTargetOrder *order = [ACPTargetOrder targetOrderWithId:@"ADCKKBC" total:@(400.50) purchasedProductIds:@[@"34", @"125"]];
ACPTargetParameters *targetParameters = [ACPTargetParameters targetParametersWithParameters:mboxParameters
                                                    profileParameters:profileParameters
                                                              product:product
                                                                order:order];

// Call the API
[ACPTarget retrieveLocationContent:requestArray withParameters:targetParameters]
```
{% endtab %}
{% endtabs %}

## Set preview restart deep link

This API sets the Target preview URL to be displayed when the preview mode is restarted.

{% tabs %}
{% tab title="Android" %}
### setPreviewRestartDeeplink

#### Syntax

```text
public static void setPreviewRestartDeepLink(final Uri deepLink)
```
{% endtab %}

{% tab title="iOS" %}
### setPreviewRestartDeeplink

#### Syntax

```objectivec
+ (void) setPreviewRestartDeeplink: (nonnull NSURL*) deeplink;
```
{% endtab %}
{% endtabs %}

## Send an mbox click notification

Sends a click notification to the configured Target server for a prefetched or regular mbox location. The click metric should be enabled for the provided location name in Target.

{% tabs %}
{% tab title="Android" %}
### locationClicked

If a notification is sent for a prefetched mbox, its contents should already have been requested with `loadRequests`, which indicates that the mbox was viewed.

#### Syntax

```java
public static void locationClicked(final String mboxName, final TargetParameters parameters)
```

#### Example

```java
// Define Mbox parameters
Map<String, Object> mboxParameters = new HashMap<>();
mboxParameters.put("membership", "prime");

// Define Product parameters
Map<String, Object> productParameters = new HashMap<>();
productParameters.put("id", "CEDFJC");
productParameters.put("categoryId","Electronics");

List<String> purchasedIds = new ArrayList<String>();
purchasedIds.add("81");
purchasedIds.add("123");
purchasedIds.add("190");

// Define Order parameters
Map<String, Object> orderParameters = new HashMap<>();
orderParameters.put("id", "NJJICK");
orderParameters.put("total", "650");
orderParameters.put("purchasedProductIds",  purchasedIds);

// Creating Profile parameters
Map<String, Object> profileParameters = new HashMap<>();
profileParameters.put("ageGroup", "20-32");

//Target Parameters
TargetOrder targetOrder = new TargetOrder("NJJICK", "650", purchasedIds);
TargetProduct targetProduct = new TargetProduct("CEDFJC", "Electronics");
TargetParameters targetParameters = new TargetParameters.Builder(mboxParameters)
                                .profileParameters(profileParameters)
                                .order(targetOrder)
                                .product(targetProduct)
                                .build();

Target.locationClicked("cartLocation", targetParameters);
```
{% endtab %}

{% tab title="iOS" %}
### locationClicked

If a notification is sent for a prefetched mbox, its contents should already have been requested with `loadRequests`, which indicates that the mbox was viewed.

#### Syntax

```objectivec
+ (void) locationClickedWithName: (nonnull NSString*) name targetParameters: (nullable ACPTargetParameters*) parameters;
```

#### Example

```objectivec
// Define Mbox parameters
NSDictionary *mboxParameters = @{@"membership":@"prime"};

// Define Product parameters
NSDictionary *productParameters = @{@"id":@"CEDFJC",
                                    @"categoryId":@"Electronics"};
// Define Order parameters
NSDictionary *orderParameters = @{@"id":@"NJJICK",
                                    @"total":@"650",
                                    @"purchasedProductIds":@"81, 123, 190"};

// Define Profile parameters
NSDictionary *profileParameters = @{@"ageGroup":@"20-32"};

// Create Target parameters
ACPTargetProduct *product = [ACPTargetProduct targetProductWithId:@"24D334" categoryId:@"Stationary"];
ACPTargetOrder *order = [ACPTargetOrder targetOrderWithId:@"ADCKKBC" total:@(400.50) purchasedProductIds:@[@"34", @"125"]];
ACPTargetParameters *targetParameters = [ACPTargetParameters targetParametersWithParameters:nil
                                                    profileParameters:nil
                                                              product:product
                                                                order:order];

[ACPTarget locationClickedWithName:@"cartLocation" targetParameters:targetParameters]
```
{% endtab %}
{% endtabs %}

## Public classes

{% tabs %}
{% tab title="Android" %}
### TargetRequest

Here is a code sample for this class in Android:

```java
public class TargetRequest extends TargetObject {

    /**
     * Instantiate a TargetRequest object
     * @param mboxName String mbox name for this request
     * @param targetParameters TargetParameters for this request
     * @param defaultContent String default content for this request
     * @param contentCallback AdobeCallback<String> which will get called with Target mbox content
     */
    public TargetRequest(final String mboxName,
                         final TargetParameters targetParameters,
                         final String defaultContent,
                         final AdobeCallback<String> contentCallback);

     /**
     * Sets mbox parameters for the request.
     *
     * @param mboxParameters Map<String, String> mbox parameters
     */
     void setMboxParameters(final Map<String, String> mboxParameters);

     /**
     * Sets profile parameters for the request.
     *
     * @param profileParameters Map<String, String profile parameters
     */
    void setProfileParameters(final Map<String, String> profileParameters);

    /**
     * Sets order parameters for the request.
     *
     * @param orderParameters Map<String, Object> order parameters
     */
    void setOrderParameters(final Map<String, Object> orderParameters);

    /**
     * Sets product parameters for the request.
     *
     * @param productParameters Map<String, String> product parameters
     */
    void setProductParameters(final Map<String, String> productParameters);

    /**
     * Sets targetParameters for the request.
     *
     * @param targetParameters TargetParameters for the request.
     */
    void setTargetParameters(final TargetParameters targetParameters);
}
```

### TargetPrefetch

Here is a code sample for this class in Android:

```java
public class TargetPrefetch extends TargetObject {

    /**
     * Instantiate a {@link TargetPrefetch} object
     * @param mboxName {@link String} mbox name for this prefetch
     * @param targetParameters {@link TargetParameters} for this prefetch
     */
     public TargetPrefetch(final String mboxName, final TargetParameters targetParameters)

     /**
     * Sets mbox parameters for the request.
     *
     * @param mboxParameters Map<String, String> mbox parameters
     */
     void setMboxParameters(final Map<String, String> mboxParameters);

     /**
     * Sets profile parameters for the request.
     *
     * @param profileParameters Map<String, String profile parameters
     */
    void setProfileParameters(final Map<String, String> profileParameters);

    /**
     * Sets order parameters for the request.
     *
     * @param orderParameters Map<String, Object> order parameters
     */
    void setOrderParameters(final Map<String, Object> orderParameters);

    /**
     * Sets product parameters for the request.
     *
     * @param productParameters Map<String, String> product parameters
     */
    void setProductParameters(final Map<String, String> productParameters);

    /**
     * Sets targetParameters for the request.
     *
     * @param targetParameters TargetParameters for the request.
     */
    void setTargetParameters(final TargetParameters targetParameters);

}
```

### TargetParameters

Here is a code sample for this class in Android:

```java
public class TargetParameters {

    private TargetParameters() {}

    /**
    * Builder used to construct a TargetParameters object
    */
    public static class Builder {
        private Map<String, String> parameters;
        private Map<String, String> profileParameters;
        private TargetProduct product;
        private TargetOrder order;

        /**
         * Create a TargetParameters object Builder
         */
        public Builder() {}

        /**
         * Create a TargetParameters object Builder
         *
         * @param parameters mbox parameters for the built TargetParameters
         */
        public Builder(final Map<String, String> parameters);

        /**
         * Set mbox parameters on the built TargetParameters
         *
         * @param parameters mbox parameters map
         * @return this builder
         */
        public Builder parameters(final Map<String, String> parameters);

        /**
         * Set profile parameters on the built TargetParameters
         *
         * @param profileParameters profile parameters map
         * @return this builder
         */
        public Builder profileParameters(final Map<String, String> profileParameters);

        /**
         * Set product parameters on the built TargetParameters
         *
         * @param product product parameters
         * @return this builder
         */
        public Builder product(final TargetProduct product);

        /**
         * Set order parameters on the built TargetParameters
         *
         * @param order order parameters
         * @return this builder
         */
        public Builder order(final TargetOrder order);

        /**
         * Build the TargetParameters object
         *
         * @return the built TargetParameters object
         */
        public TargetParameters build();
    }
}
```

### TargetOrder

Here is a code sample for this class in Android:

```java
public class TargetOrder {

    /**
     * Initialize a TargetOrder with an order id, order total and a list of purchasedProductIds
     *
     * @param id String order id
     * @param total double order total amount
     * @param purchasedProductIds a list of purchased product ids
     */
    public TargetOrder(final String id, final double total, final List<String> purchasedProductIds);
    /**
     * Get the order id
     *
     * @return order id
     */
    public String getId();

    /**
     * Get the order total
     *
     * @return order total
     */
    public double getTotal();

    /**
     * Get the order purchasedProductIds
     *
     * @return a list of this order's purchased product ids
     */
    public List<String> getPurchasedProductIds();

    /**
     * Converts an order parameter Map to a TargetOrder
     *
     * @param orderParameters a Map<String, Object> of Target order parameters
     * @return converted TargetOrder
     */
    static TargetOrder fromMap(final Map<String, Object> orderParameters);

    /**
     * Converts TargetOrder to an order parameters Map.
     *
     * @param targetOrder a TargetOrder object
     * @return Map<String, Object> containing Target order parameters
     */
    static Map<String, Object> toMap(final TargetOrder targetOrder);

}
```

### TargetProduct

Here is a code sample for this class in Android:

```java
public class TargetProduct {

    /**
     * Initialize a TargetProduct with a product id and a productCategoryId categoryId
     *
     * @param id String product id
     * @param categoryId String product category id
     */
     public TargetProduct(final String id, final String categoryId);

    /**
     * Get the product id
     *
     * @return product id
     */
    public String getId();

    /**
     * Get the product categoryId
     *
     * @return product category id
     */
    public String getCategoryId();

    /**
     * Converts a product parameter Map to a TargetProduct
     *
     * @param productParameters a Map<String, String> of Target product parameters
     * @return converted TargetProduct
     */
    static TargetProduct fromMap(final Map<String, String> productParameters);

    /**
     * Converts a TargetProduct object to product parameters Map.
     *
     * @param targetProduct a TargetProduct object
     * @return Map<String, String> containing Target product parameters
     */
     static Map<String, String> toMap(final TargetProduct targetProduct);
}
```
{% endtab %}

{% tab title="iOS" %}
### ACPTargetRequestObject

This class extends `ACPTargetPrefetchObject` by adding default content and a function pointer property that will be used as a callback when requesting content from Target:

```objectivec
@interface ACPTargetRequestObject : ACPTargetPrefetchObject

///< The default content that will be returned if Target servers are unreachable    
@property(nonatomic, strong, nonnull) NSString* defaultContent;

///< Optional. When batch requesting Target locations, callback will be invoked when content is available for this location.
@property(nonatomic, strong, nullable) void (^callback)(NSString* __nullable content);
@end
```

The following method can be used to create an instance of a Target prefetch object that might be used to make a batch request to the configured Target server to prefetch content for mbox locations:

```text
+ (nonnull instancetype) targetRequestObjectWithName: (nonnull NSString*) name
                                    targetParameters: (nullable ACPTargetParameters*) targetParameters
                                      defaultContent: (nonnull NSString*) defaultContent
                                            callback: (nullable void (^) (NSString* __nullable content)) callback;
```

### ACPTargetPrefetchObject

This class contains the name of the Target location/mbox and parameter dictionary for mbox parameters, product parameters, and order parameters that will be used in a prefetch:

```objectivec
@interface ACPTargetPrefetchObject : NSObject

///< The name of the Target location/mbox
@property(nonatomic, strong, nullable) NSString* name;

///target parameters associated with the prefetch object. You can set all other parameters in this object
@property(nonatomic, strong, nullable) ACPTargetParameters* targetParameters;
@end
```

The following method can be used to create an instance of a Target prefetch object that might be used to make a batch request to the configured Target server to prefetch content for mbox locations:

```text
+ (nonnull instancetype) targetPrefetchObjectWithName: (nonnull NSString*) name
                                     targetParameters: (nullable ACPTargetParameters*) targetParameters;
```

### ACPTargetParameters

This class contains the parameter dictionary for mbox parameters, profile parameters, and ACPTargetOrder object as well as ACPTargetProduct object:

```objectivec
@interface ACPTargetParameters : NSObject

///Dictionary containing key-value pairs of parameters
@property(nonatomic, strong, nullable) NSDictionary<NSString*, NSString*>* parameters;

///Dictionary containing key-value pairs of profile parameters
@property(nonatomic, strong, nullable) NSDictionary<NSString*, NSString*>* profileParameters;

///ACPTargetOrder object
@property(nonatomic, strong, nullable) ACPTargetOrder* order;

///ACPTargetProduct object
@property(nonatomic, strong, nullable) ACPTargetProduct* product;
@end
```

The following method can be used to create an instance of a Target parameters object that can be used when invoking multiple APIs:

```text
+ (nonnull instancetype) targetParametersWithParameters: (nullable NSDictionary*) parameters
                                      profileParameters: (nullable NSDictionary*) profileParameters
                                                product: (nullable ACPTargetProduct*) product
                                                  order: (nullable ACPTargetOrder*) order;
```

### ACPTargetOrder

This class contains orderId, total and array for purchasedProductIds present in order parameters:

```objectivec
@interface ACPTargetOrder : NSObject

///Order ID
@property(nonatomic, strong, nonnull) NSString* orderId;

///Order total
@property(nonatomic, strong, nullable) NSNumber* total;

///Array of Purchased Product Ids
@property(nonatomic, strong, nullable) NSArray<NSString*>* purchasedProductIds;
@end
```

The following method can be used to create an instance of a Target order object that can be used when invoking multiple APIs:

```text
+ (nonnull instancetype) targetOrderWithId: (nonnull NSString*) orderId
                                     total: (nullable NSNumber*) total
                       purchasedProductIds: (nullable NSArray <NSString*>*) purchasedProductIds;
```

### ACPTargetProduct

This class contains productId and categoryId present in product parameters:

```objectivec
@interface ACPTargetProduct : NSObject

///Product ID
@property(nonatomic, strong, nullable) NSString* productId;

///Category ID
@property(nonatomic, strong, nullable) NSString* categoryId;
@end
```

The following method can be used to create an instance of a Target Product object that can be used when invoking multiple APIs:

```text
+ (nonnull instancetype) targetProductWithId: (nonnull NSString*) productId
                                  categoryId: (nullable NSString*) categoryId;
```
{% endtab %}
{% endtabs %}

