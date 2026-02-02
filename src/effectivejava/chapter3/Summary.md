# Methods common to all objects

**Item10**: Obey the general contract when overriding equals
1. Use the == operator to check if the argument is a reference to this object
2. Use the instanceof operator to check if the argument has the correct type.
3. Cast the argument to correct type
4. For each "significant" field in the class, check if that field of the argument matches the 
corresponding field of this object

When you are finished writing your equals method, ask yourself three questions: Is it symmetric? Is it transitive? Is it consistent? 

// Class with a typical equals method
public final class PhoneNumber {
    private final short areaCode, prefix, lineNum;

    public PhoneNumber(int areaCode, int prefix, int lineNum) {
        this.areaCode = rangeCheck(areaCode,  999, "area code");
        this.prefix   = rangeCheck(prefix,    999, "prefix");
        this.lineNum  = rangeCheck(lineNum,  9999, "line num");
    }

    private static short rangeCheck(int val, int max, String arg) {
        if (val < 0 || val > max)
           throw new IllegalArgumentException(arg + ": " + val);
        return (short) val;
    }

    @Override public boolean equals(Object o) {
        if (o == this)
            return true;
        if (!(o instanceof PhoneNumber))
            return false;
        PhoneNumber pn = (PhoneNumber)o;
        return pn.lineNum == lineNum && pn.prefix == prefix
                && pn.areaCode == areaCode;
    }
    ... // Remainder omitted
}

While there is no satisfactory way to extend an instantiable class and add a value component, there is a fine workaround: Follow the advice of Item 18, “Favor composition over inheritance.”

// Adds a value component without violating the equals contract
public class ColorPoint {
   private final Point point;
   private final Color color;

   public ColorPoint(int x, int y, Color color) {
      point = new Point(x, y);
      this.color = Objects.requireNonNull(color);
   }

   /**
    * Returns the point-view of this color point.
    */
   public Point asPoint() {
      return point;
   }

   @Override public boolean equals(Object o) {
      if (!(o instanceof ColorPoint))
         return false;
      ColorPoint cp = (ColorPoint) o;
      return cp.point.equals(point) && cp.color.equals(color);
   }

   ...    // Remainder omitted
}

*Few final caveats*
1. Always override hashcode when you override equals
2. Don't substitute another type for Object in the equals declaration

// Broken - parameter type must be Object!
public boolean equals(MyClass o) {
    ...
}

