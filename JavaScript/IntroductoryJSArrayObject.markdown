# Array Object Exercises (JavaScript)

## Questions

1. If you have an array `var a = [ 0, 1, 2, 3, 4 ];`, how do you extract the value 3 from the array?

2. If you have the following object, how do you extract the value 3 from the object?

    ```
    var a = {
        "zero": 0,
        "one": 1,
        "two": 2,
        "three": 3,
        "four": 4
    };
    ```

3. If you have the following array, how do you extract the value 3 from the array?

    ```
    var a = [
        [
            0,
            1
        ],
        [
            2,
            [
                3
            ]
        ]
    ];
    ```

4. If you have the following object, how do you extract the value 3 from the object?

    ```
    var a = {
        "a": {
            "b": 0,
            "c": 1
        },
        "b": {
            "e": 2,
            "o": {
                "b": 3
            }
        }
    };
    ```

5. Create a new array with each comma-separated value as its own array element from the string `var a = "a,b,c,d,e,f"`.

6. With the result array from `5`, create a new object where each element is *both* key *and* value. In other words, the result should be:

    ```
    {
        "a": "a",
        "b": "b",
        "c": "c",
        "d": "d",
        "e": "e"
    }
    ```

7. You have two objects like the following. One contains field labels, the other contains field values. Write a program to output the third object.

    ```
    var keys = {
        "field1": "first",
        "field2": "second",
        "field3": "third"
    };
    var values = {
        "field1value": "dinosaur",
        "field2value": "pig",
        "field3value": "platypus"
    };

    // want to output
    var keysAndValues = {
        "first": "dinosaur",
        "second": "pig",
        "third": "platypus"
    };
    ```

8. You have an array of transactions, each of which has a debit and credit amount. Find the absolute value of the transaction amount (i.e. `abs( debit - credit )`) and add it as a new `key=>value` pair to each transaction.

    ```
    var transactions = [ {
        "debit": 2,
        "credit": 3
    }, {
        "debit": 15,
        "credit": 14
    } ];

    // outputs
    transactions = [ {
        "debit": 2,
        "credit": 3,
        "amount": 1
    }, {
        "debit": 15,
        "credit": 14,
        "amount": 1
    } ];
    ```

9. Find the sum of this array of numbers `var a = [ 0, 1, 2, 3, 4, 5, 6 ];`.

## Sample Answers (others may be correct :-D)

1. `a[ 3 ];`

2. `a.three;`

3. `a[1][1][0];`

4. `a.b.o.b;`

5. `a = a.split( "," );`

6. Samples:

    ```
    var b = {};
    a.forEach( function( value, index ) {
        b[ value ] = value;
    } );
    ```

    **or**

    ```
    var b = {};
    for ( var i = 0;  i < a.length; i++ ) {
        b[ a[ i ] ] = a[ i ];
    }
    ```

7. Samples:

    ```
    $keysAndValues = {};
    keysAndValues[ keys.field1 ] = values.field1value;
    keysAndValues[ keys.field2 ] = values.field2value;
    keysAndValues[ keys.field3 ] = values.field3value;
    ```

8. Samples

    ```
    transactions.map( function( transaction ) {
        return ( transaction.amount = Math.abs( transaction.debit - transaction.credit ) );
    } );

    ```

    **or**

    ```
    transactions.forEach( function( transaction ) {
        transaction.amount = Math.abs( transaction.debit - transaction.credit );
    } );
    ```

9. Samples:

    ```
    var sum = 0;
    var sum = a.reduce( function( x, y ) {
        return x + y;
    }, 0 );
    ```

    **or**

    ```
    a.forEach( function( value ) {
        sum += value;
    } );
    ```

    **or**

    ```
    var sum = 0;
    for ( var i = 0; i < a.length; i++ ) {
        sum += a[ i ];
    }
    ```
