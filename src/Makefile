CFLAGS=-g -Wall -Werror -Wextra 
LFLAGS=-lcheck -lpthread -lsubunit -lm
GCOV_FLAGS=-fprofile-arcs -ftest-coverage -fPIC
CFILES=s21_matrix.c
CEXE=test
VALGRIND_FLAGS  = 	--log-file="valgrind.txt" --tool=memcheck --leak-check=yes --track-origins=yes --leak-check=full

.PHONY: dist clean

all:
	gcc -Wall -Werror -Wextra  build/main.c gui/cli/front.c brick_game/tetris/back.c -lncurses
	
test: 
	gcc ${CFLAGS} tests/*.c brick_game/tetris/back.c  gui/cli/front.c -o s21_test -lncurses $(LFLAGS)
	./s21_test

back.a:
	gcc ${CFLAGS} -c brick_game/tetris/back.c -lncurses
	ar rc back.a *.o
	ranlib back.a
	rm -f *.o

clean:
	rm -rf *.a *.o *.gcno *.gcda test *.info s21_test report valgrind.txt tetris dist
	rm -rf htmlmake doc

gcov_report: clean
	gcc ${CFLAGS} tests/*.c brick_game/tetris/back.c  gui/cli/front.c -o s21_test -lncurses $(LFLAGS) ${GCOV_FLAGS}
	./s21_test
	lcov -t "gcov_test" -o gcov_test.info -c -d .
	genhtml -o report gcov_test.info
	xdg-open report/index.html
	rm -rf *.gcda *.gcno *.info

install: 
	gcc -Wall -Werror -Wextra  build/main.c gui/cli/front.c brick_game/tetris/back.c -lncurses -o tetris

valgrind: clean test
	valgrind $(VALGRIND_FLAGS) ./s21_test
	grep -n "ERROR SUMMARY" valgrind.txt

uninstall: 
	rm -rf tetris

dvi: clean
	doxygen dvi_config.txt
	xdg-open doc/html/index.html

dist: install
	mkdir -p dist
	cp -r Makefile brick_game build gui tetris dist/
	tar -ckzvf dist/tetris$(shell date +%Y%m%d).tar.gz -C dist .
	cd dist && rm -rf Makefile 
clang: 
	clang-format -n tests/*.c tests/*.h brick_game/tetris/*.c  build/*.c build/*.h gui/cli/*.c