cmake_minimum_required(VERSION 3.10)

project(building-an-interpreter-a-repl-calculator LANGUAGES C)

add_executable(calculator calculator.c)

enable_testing()

function(do_test id expr will_fail)
    if(NOT EXISTS(test_file_${id}))
        write_file(test_file_${id} ${expr})
    endif()
    add_test(NAME test_${id} COMMAND calculator test_file_${id})
    set_tests_properties(test_${id} PROPERTIES WILL_FAIL ${will_fail})
endfunction()

do_test(1 "2+2" false)
do_test(2 "(2+2))" true) # extra bracket
do_test(3 "5+10+56\n4+32" false)
do_test(4 "52 52+36" true) # syntax error
do_test(5 "43++45" true) # syntax error
do_test(6 "1+2-(3*4)/5" false)
do_test(7 "8" false)
do_test(8 "1+2=3" true) # unknown character
do_test(9 "10/(45/9-5)" true) # runtime error (division by 0)