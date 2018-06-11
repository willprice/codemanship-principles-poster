# 2. Code that is easy to understand

Code should be easy to understand, otherwise it is hard to change and adapt to
changing requirements. Complex code takes longer to understand and limits the
number of people able to change it.

Aspects that make code easy to understand:

* Well chosen names
* Simple methods
* Objects having only one purpose
* Lack of gratuitous abstraction 

## Bad example

```java
public class {
  
}
```

## Good example

```java
```


# 4. Code that is made of the simplest parts

Code should be split into the smallest simplest chunks possible as this improves
verifiability and composition.

## Bad example

```java
public class StockPrice {
    public currentPrice() {
        return Float.parseFloat(getUrl("http://currency-exchange.com/gbp-to-usd")) * 
            Float.parseFloat(getUrl("http://stockmarket.co.uk/" + shortName));
    }
}
```

## Good example

```java
public RemotePrice {
    public RemotePrice(URL location);
    public float fetch();
}

public CurrencyConverter {
    public convert(Currency from, Currency to);
}

public class StockPrice {
    RemotePrice remotePrice;
    CurrencyConverter currencyConverter;
    Currency currency;

    public currentPrice() { remotePrice.fetch() * currencyConverter(remotePrice.currency, currency); }
}
```


# 6. Code should tell, don't ask

Computation should happen where the data is held, rather than obtained and then
done locally--this encourages lower coupling and higher cohesion amongst objects

## Bad example

```java
public class ShoppingCart {
    public void checkout() {
        ...
        String address = String.join("\n", customer.getName(),
            customer.firstLineAddress(), customer.secondLineAddress(),
            customer.city(), customer.region(), customer.postCode()
        );
        ...
    }
}
```

## Good example

```java
public class ShoppingCart {
    public void checkout() {
        ...
        String address = customer.getFullAddress();
        ...
    }
}
```


# 8. Code should present client-specific interfaces

Code that uses an object that has many responsibilities shouldn't depend upon
that object, but upon an interface that exposes the minimal methods and
attributes that enable the necessary interactions between the user code and the
client. This encourages low coupling between components.

## Bad example

```java
public class Image {
  public byte[] serialize() { ... }
}

public class FileStore {
    public void saveImage(Image img, Path path) {
        byte[] data = img.serialize()
        FileOutputStream stream = new FileOutputStream(path);
        try {
            stream.write(data);
        } finally {
            stream.close()
        }
    }
}
```

## Good example

```java
public interface ByteSerializable {
    byte[] toBytes();
}

public class Image implements ByteSerializable {
    @Override
    public byte[] toBytes() { ... }
}

public class Video implements ByteSerializable {
    @Override
    public byte[] toBytes() { ... }
}

public class FileStore {
    public void saveImage(ByteSerializable obj, Path path) {
        byte[] data = obj.toBytes()
        FileOutputStream stream = new FileOutputStream(path);
        try {
            stream.write(data);
        } finally {
            stream.close()
        }
    }
}
```
```

