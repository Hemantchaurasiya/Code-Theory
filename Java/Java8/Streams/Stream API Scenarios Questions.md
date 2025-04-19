// Java Stream API Scenario-Based Questions and Solutions

import java.util.*;
import java.util.function.Function;
import java.util.stream.*;

public class StreamApiScenarios {

      // 1. Convert a list of strings into a list of their lengths
      public static List<Integer> stringLengths(List<String> input) {
          return input.stream()
                      .map(String::length)
                      .collect(Collectors.toList());
      }
  
      // 2. Find square of even numbers and collect into a list
      public static List<Integer> squareEvenNumbers(List<Integer> numbers) {
          return numbers.stream()
                        .filter(n -> n % 2 == 0)
                        .map(n -> n * n)
                        .collect(Collectors.toList());
      }
  
      // 3. Remove duplicates from a list using streams
      public static List<Integer> removeDuplicates(List<Integer> numbers) {
          return numbers.stream()
                        .distinct()
                        .collect(Collectors.toList());
      }

    // 4. Filter out null and empty strings
    public static List<String> filterNullEmpty(List<String> input) {
        return input.stream()
                    .filter(s -> s != null && !s.isEmpty())
                    .collect(Collectors.toList());
    }

    // 5. Convert list of strings to uppercase
    public static List<String> toUpperCaseList(List<String> input) {
        return input.stream()
                    .map(String::toUpperCase)
                    .collect(Collectors.toList());
    }

    // Sample Employee class for grouping and other operations
    static class Employee {
        String name;
        String department;
        double salary;

        Employee(String name, String department, double salary) {
            this.name = name;
            this.department = department;
            this.salary = salary;
        }

        public String getName() { return name; }
        public String getDepartment() { return department; }
        public double getSalary() { return salary; }

        @Override
        public String toString() {
            return name + "(" + department + ", $" + salary + ")";
        }
    }

    // 6. Group employees by department
    public static Map<String, List<Employee>> groupByDepartment(List<Employee> employees) {
        return employees.stream()
                        .collect(Collectors.groupingBy(Employee::getDepartment));
    }

    // 7. Count number of employees in each department
    public static Map<String, Long> countByDepartment(List<Employee> employees) {
        return employees.stream()
                        .collect(Collectors.groupingBy(Employee::getDepartment, Collectors.counting()));
    }

    // 8. Find highest salaried employee in each department
    public static Map<String, Optional<Employee>> highestSalaryInDepartment(List<Employee> employees) {
        return employees.stream()
                        .collect(Collectors.groupingBy(
                            Employee::getDepartment,
                            Collectors.maxBy(Comparator.comparing(Employee::getSalary))));
    }

    // 9. Sort list of employees by salary
    public static List<Employee> sortBySalary(List<Employee> employees) {
        return employees.stream()
                        .sorted(Comparator.comparing(Employee::getSalary))
                        .collect(Collectors.toList());
    }

    // 10. Calculate average salary of all employees
    public static double averageSalary(List<Employee> employees) {
        return employees.stream()
                        .mapToDouble(Employee::getSalary)
                        .average()
                        .orElse(0.0);
    }

    // 11. Use reduce() to calculate sum of integers
    public static int sumWithReduce(List<Integer> numbers) {
        return numbers.stream()
                      .reduce(0, Integer::sum);
    }

    // 12. Difference between reduce and collect
    public static void reduceVsCollectExample() {
        List<String> list = Arrays.asList("a", "b", "c");

        // Using reduce
        String reduced = list.stream().reduce("", String::concat);

        // Using collect
        String collected = list.stream().collect(Collectors.joining());

        System.out.println("Reduced: " + reduced);
        System.out.println("Collected: " + collected);
    }

    // 13. Join strings with comma
    public static String joinStrings(List<String> items) {
        return items.stream()
                    .collect(Collectors.joining(", "));
    }

    // 14. Partition employees by salary > 50000
    public static Map<Boolean, List<Employee>> partitionBySalary(List<Employee> employees) {
        return employees.stream()
                        .collect(Collectors.partitioningBy(e -> e.getSalary() > 50000));
    }

    // 15. Collect employees into map by department
    public static Map<String, List<Employee>> employeesByDept(List<Employee> employees) {
        return employees.stream()
                        .collect(Collectors.groupingBy(Employee::getDepartment));
    }

    // 16. Difference between map and flatMap
    public static List<String> mapVsFlatMapExample(List<String> sentences) {
        // map example - produces Stream<Stream<String>>
        List<Stream<String>> mapped = sentences.stream()
                                               .map(s -> Arrays.stream(s.split(" ")))
                                               .collect(Collectors.toList());

        // flatMap example - produces Stream<String>
        List<String> flatMapped = sentences.stream()
                                           .flatMap(s -> Arrays.stream(s.split(" ")))
                                           .collect(Collectors.toList());

        return flatMapped;
    }

    // 17. Flatten list of lists of integers
    public static List<Integer> flattenListOfLists(List<List<Integer>> listOfLists) {
        return listOfLists.stream()
                          .flatMap(Collection::stream)
                          .collect(Collectors.toList());
    }

    // 18. Extract words from list of sentences
    public static List<String> wordsFromSentences(List<String> sentences) {
        return sentences.stream()
                        .flatMap(s -> Arrays.stream(s.split(" ")))
                        .collect(Collectors.toList());
    }

    // 19. Using parallel stream
    public static int sumWithParallelStream(List<Integer> numbers) {
        return numbers.parallelStream()
                      .reduce(0, Integer::sum);
    }

    // 20. Thread safety with parallel stream
    public static List<Integer> threadSafeParallelProcessing(List<Integer> numbers) {
        return numbers.parallelStream()
                      .map(n -> n * 2)
                      .collect(Collectors.toList());
    }

    // 21. Skip and limit to paginate list
    public static List<Integer> paginate(List<Integer> list, int page, int size) {
        return list.stream()
                   .skip((long) (page - 1) * size)
                   .limit(size)
                   .collect(Collectors.toList());
    }

    // 22. Find second highest number
    public static Optional<Integer> secondHighest(List<Integer> numbers) {
        return numbers.stream()
                      .distinct()
                      .sorted(Comparator.reverseOrder())
                      .skip(1)
                      .findFirst();
    }

    // 23. Count frequency of elements
    public static Map<String, Long> countFrequencies(List<String> words) {
        return words.stream()
                    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    }

    // 24. Check if all elements match a condition
    public static boolean allEven(List<Integer> numbers) {
        return numbers.stream().allMatch(n -> n % 2 == 0);
    }

    // 25. Check if any element matches a condition
    public static boolean anyNegative(List<Integer> numbers) {
        return numbers.stream().anyMatch(n -> n < 0);
    }

    // 26. Find first non-repeated character in a string
    public static Optional<Character> firstNonRepeatedChar(String input) {
        Map<Character, Long> freq = input.chars()
                                         .mapToObj(c -> (char) c)
                                         .collect(Collectors.groupingBy(Function.identity(), LinkedHashMap::new, Collectors.counting()));
        return freq.entrySet().stream()
                   .filter(e -> e.getValue() == 1)
                   .map(Map.Entry::getKey)
                   .findFirst();
    }

    // 27. Filter and transform using method reference
    public static List<String> filterAndUppercase(List<String> input) {
        return input.stream()
                    .filter(s -> s != null && !s.isEmpty())
                    .map(String::toUpperCase)
                    .collect(Collectors.toList());
    }

    // 28. Convert comma-separated string to list of integers
    public static List<Integer> parseCommaSeparated(String input) {
        return Arrays.stream(input.split(","))
                     .map(String::trim)
                     .map(Integer::parseInt)
                     .collect(Collectors.toList());
    }

    // 29. Sort strings ignoring case
    public static List<String> sortIgnoreCase(List<String> input) {
        return input.stream()
                    .sorted(String.CASE_INSENSITIVE_ORDER)
                    .collect(Collectors.toList());
    }

    // 30. Convert a map to a list of formatted strings
    public static List<String> mapToFormattedStrings(Map<String, Integer> map) {
        return map.entrySet().stream()
                  .map(e -> e.getKey() + "=" + e.getValue())
                  .collect(Collectors.toList());
    }
}
