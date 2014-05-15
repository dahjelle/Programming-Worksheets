# Array Exercises (PHP)

## Questions

1. If you have an array `$a = array( 0, 1, 2, 3, 4 );`, how do you extract the value 3 from the array?

2. If you have an array `$a = array( "zero"=>0, "one"=>1, "two"=>2, "three"=>3, "four"=>4 );`, how do you extract the value 3 from the array?

3. If you have the following array, how do you extract the value 3 from the array?

    ```
    $a = array(
        array(
            0,
            1
        ),
        array(
            2,
            array(
                3
            )
        )
    );
    ```

4. If you have the following array, how do you extract the value 3 from the array?

    ```
    $a = array(
        "a"=>array(
            "b"=>0,
            "c=>1
        ),
        "b"=>array(
            "e"=>2,
            "o"=>array(
                "b"=>3
            )
        )
    );
    ```

5. Create a new array with each comma-separated value as its own array element from the string `$a = "a,b,c,d,e,f"`.

6. With the result array from `5`, create a new array where each element is *both* key *and* value. In other words, the result should be:

    ```
    array(
        "a"=>"a",
        "b"=>"b",
        "c"=>"c",
        "d"=>"d",
        "e"=>"e"
    )
    ```

7. You have two arrays like the following. One contains field labels, the other contains field values. Write a program to output the third array.

    ```
    $keys = array(
        "field1"=>"first",
        "field2"=>"second",
        "field3"=>"third"
    );
    $values = array(
        "field1value"=>"dinosaur",
        "field2value"=>"pig",
        "field3value"=>"platypus"
    );
    // want to output
    $keysAndValues = array(
        "first"=>"dinosaur",
        "second"=>"pig",
        "third"=>"platypus"
    );
    ```

8. You have an array of transactions, each of which has a debit and credit amount. Find the absolute value of the transaction amount (i.e. `abs( debit - credit )`) and add it as a new `key=>value` pair to each transaction.

    ```
    $transactions = array(
        array(
            "debit"=>2,
            "credit"=>3
        ),
        array(
            "debit"=>15,
            "credit"=>14
        )
    );
    // outputs
    $transactions = array(
        array(
            "debit"=>2,
            "credit"=>3,
            "amount"=>1
        ),
        array(
            "debit"=>15,
            "credit"=>14,
            "amount"=>1
        )
    );
    ```

9. Find the sum of this array of numbers `$a = array( 0, 1, 2, 3, 4, 5, 6 );`.

## Sample Answers (others may be correct :-D)

1. `$a[ 3 ];`

2. `$a[ "three" ];`

3. `$a[ 1 ][ 1 ][ 0 ];`

4. `$a[ "b" ][ "o" ][ "b" ];`

5. `$a = explode( ",", $a );`

6. Samples:

    ```
    $new_array = array();
    foreach( $a as $element ) {
        $new_array[ $element ] = $element;
    }
    ```

    **or**

    `array_combine( $a, $a );`

    How would you write your own `array_combine` function?

7. Samples:

    ```
    // can you spot the bug?
    $keysAndValues = array();
    $keysAndValues[ $keys[ "field1" ] ] = $values[ "field1value" ];
    $keysAndValues[ $keys[ "field2" ] ] = $values[ "field2value" ];
    $keysAndValues[ $keys[ "field3" ] ] = $values[ "field2value" ];
    ```

    **or**

    ```
    $keysAndValues = array();
    foreach( array( 1, 2, 3 ) as $index ) {
        $keysAndValues[ $keys[ "field$index" ] ] = $values[ "field" . $index . "value" ];
    }
    ```

    **or**

    ```
    $keysAndValues = array_combine( array_values( $keys ), array_values( $values ) );
    ```

8. Samples

    ```
    $new_transactions = array();
    foreach( $transactions as $transaction ) {
        $new_transaction = $transaction;
        $new_transaction[ "amount" ] = abs( $transaction[ "debit" ] - $transaction[ "credit" ] );
        $new_transactions[] = $new_transaction;
    }
    $transactions = $new_transactions;
    ```

    **or**

    ```
    $transactions = array_map( function( $transaction ) {
        $transaction[ "amount" ] = abs( $transaction[ "debit" ] - $transaction[ "credit" ] );
        return $transaction;
    }, $transactions );
    ```

9. Samples:

    ```
    $sum = 0;
    foreach( $a as $element ) {
        $sum += $element;
    }
    ```

    **or**

    ```
    $sum = array_reduce( $a, function( $partial_sum, $element ) {
        return $partial_sum + $element;
    }, 0 );
    ```

    **or**

    ```
    $sum = array_sum( $a );
    ```
