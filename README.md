# Nberry-20.11_-p.a.
package chapter_20.11;

import java.io.File;
import java.io.IOException;
import java.nio.file.Files;
import java.util.LinkedList;

/**
 * (Match grouping symbols) A Java program contains various pairs of grouping
 * symbols, such as:
 *
 * - Parentheses: ( and )
 * - Braces: { and }
 * - Brackets: [ and ]
 *
 * Note that the grouping symbols cannot overlap. For example, (a{b)} is illegal.
 * Write a program to check whether a Java source-code file has correct pairs of
 * grouping symbols. Pass the source-code file name as a command-line argument.
 */
public class PE_20_11_Match_grouping_symbols {
    /**
     * Main method to check the correctness of grouping symbols in a Java source-code file.
     * @param args Command-line arguments. Expects a single argument - the file name.
     */
    public static void main(String[] args) {
        try {
            validateArgs(args);
            File file = new File(args[0]);
            String fileData = new String(Files.readAllBytes(file.toPath()));
            String result = isGroupedCorrectly(fileData) ? "has " : "does not have ";
            System.out.println("The file " + result + "the correct pairs of grouping symbols");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * Checks if a given character is a closing grouping symbol.
     * @param ch The character to check.
     * @return True if the character is a closing grouping symbol, false otherwise.
     */
    private static boolean isCloser(char ch) {
        return ch == ')' || ch == ']' || ch == '}';
    }

    /**
     * Checks if the grouping symbols in the file are correct and not overlapping.
     * @param fileData The content of the file.
     * @return True if the grouping symbols are correct, false otherwise.
     */
    private static boolean isGroupedCorrectly(String fileData) {
        LinkedList<Character> stack = new LinkedList<>();
        for (int i = 0; i < fileData.length(); i++) {
            Character character = fileData.charAt(i);
            if (isOpener(character)) {
                stack.push(character);
            } else if (isCloser(character)) {
                if (stack.isEmpty()) return false;
                if (isMatch(stack.peek(), character)) {
                    stack.pop();
                } else return false;
            }
        }
        return stack.isEmpty();
    }

    /**
     * Checks if a given opening and closing symbol form a correct pair.
     * @param opener The opening symbol.
     * @param closer The closing symbol.
     * @return True if the pair is correct, false otherwise.
     */
    private static boolean isMatch(char opener, char closer) {
        return (opener == '(' && closer == ')') ||
                (opener == '[' && closer == ']') ||
                (opener == '{' && closer == '}');
    }

    /**
     * Checks if a given character is an opening grouping symbol.
     * @param ch The character to check.
     * @return True if the character is an opening grouping symbol, false otherwise.
     */
    private static boolean isOpener(char ch) {
        return ch == '(' || ch == '[' || ch == '{';
    }

    /**
     * Validates the command-line arguments. Expects exactly one argument - the file name.
     * @param args Command-line arguments.
     * @throws IOException If the number of arguments is not equal to 1.
     */
    private static void validateArgs(String[] args) throws IOException {
        if (args.length != 1) {
            throw new IOException("Wrong number of arguments");
        }
    }
}
