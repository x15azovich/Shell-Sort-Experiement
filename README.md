# Shell-Sort-Experiement
import edu.princeton.cs.introcs.StdOut;
import edu.princeton.cs.introcs.StdRandom;

public class ShellSortDiagnostics {
	public static int numCompares = 0;

	public static void main(String[] args) {
		File f1 = new File();
		final int REPS = 1000000;// create a million arrays
		Integer[] a = new Integer[100];
		for (int i = 1; i <= 100; i++)
			a[i - 1] = i;
		long totalCompares = 0;
		int low = Integer.MAX_VALUE, high = 0;
		for (int times = 0; times < REPS; times++) {
			numCompares = 0;
			StdRandom.shuffle(a);
			// shuffle the arrays and then print out each array in a text file
			// to see the data
			for (int i = 0; i < 100; i++)
				f1.Append("|" + a[i] + "|" + " ");

			sort(a);// sort the arrays and print out the number of compares a
					// few spaces after the given array
			if (numCompares < low)
				low = numCompares;
			if (numCompares > high)
				high = numCompares;
			totalCompares += numCompares;
			f1.Append("                                                                                                                                   ");
			f1.Append("                                [" + numCompares
					+ "] \n" + "\n");
		}
		double avgCompares = (double) totalCompares / REPS;
		// print out the highest, lowest, and average rates
		System.out.println("Low: " + low + " High: " + high + " Average: "
				+ (int) avgCompares + " Reps: " + REPS);

	}

	// method to comare
	public static void sort(Comparable[] a) {
		int N = a.length;

		// 3x+1 increment sequence: 1, 4, 13, 40, 121, 364, 1093, ...
		int h = 1;
		while (h < N / 3)
			h = 3 * h + 1;

		while (h >= 1) {
			// h-sort the array
			for (int i = h; i < N; i++) {
				for (int j = i; j >= h && less(a[j], a[j - h]); j -= h) {
					exch(a, j, j - h);
				}
			}
			assert isHsorted(a, h);
			h /= 3;
		}
		assert isSorted(a);
	}

	/***********************************************************************
	 * Helper sorting functions
	 ***********************************************************************/

	// is v < w ?
	private static boolean less(Comparable v, Comparable w) {
		// add the compares
		numCompares++;
		return (v.compareTo(w) < 0);
	}

	// exchange a[i] and a[j]
	private static void exch(Object[] a, int i, int j) {
		Object swap = a[i];
		a[i] = a[j];
		a[j] = swap;
	}

	/***********************************************************************
	 * Check if array is sorted - useful for debugging
	 ***********************************************************************/
	private static boolean isSorted(Comparable[] a) {
		for (int i = 1; i < a.length; i++)
			if (less(a[i], a[i - 1]))
				return false;
		return true;
	}

	// is the array h-sorted?
	private static boolean isHsorted(Comparable[] a, int h) {
		for (int i = h; i < a.length; i++)
			if (less(a[i], a[i - h]))
				return false;
		return true;
	}

	// print array to standard output
	private static void show(Comparable[] a) {
		for (int i = 0; i < a.length; i++) {
			StdOut.println(a[i]);
		}
	}

}

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;


public class File {
	
	public void Append(String array) {
		try (PrintWriter out = new PrintWriter(new BufferedWriter(
				new FileWriter("PS5.txt", true)))) {
			out.print(array);
			
		} catch (IOException e) {
			// exception handling left as an exercise for the reader
		}
	}
}


