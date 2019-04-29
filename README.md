# Data Driven Tests

It is simple header-only library for Data Driven tests.

Basic usage example:

```c++
#include <sstream>
#include "DataDrivenTest.hpp"

int factorial(int n) {
  int res = 1;
  for (int i = 2; i <= n; ++i) {
    res *= i;
  }
  return res;
}

const struct {
  int input;
  int expected;
} test_data[] = {
  { 1, 1 },
  { 2, 2 },
  { 3, 6 },
  { 4, 24 },
  { 5, 120 },
};

int main()
{
  DataDrivenTest ddt;

	// configure
  ddt.config_show_passed(false);
	ddt.config_debug_break(false);

  // simple test case
  ddt.test_case("simple test case", [&](DataDrivenTest::TestCase& tc) {
    tc.assert_equal(2, 1 + 1, "1 + 1");
    tc.assert_equal(3, 1 + 2, "1 + 2");
  });

  // loop of test cases
  for (auto& item : test_data) {
    std::ostringstream test_name;
    
    test_name << "Test: factorial(" << item.input << ")";
    ddt.test_case(test_name.str(), [&](DataDrivenTest::TestCase& tc) {
      tc.assert_equal(item.expected, factorial(item.input), "factorial");
    });
  }

  return ddt.result();
}
```
