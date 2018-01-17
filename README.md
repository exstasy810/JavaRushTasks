package com.javarush.task.task16.task1630;

import java.io.*;
import java.nio.file.Paths;
import java.util.Scanner;

public class Solution {
    public static String firstFileName;
    public static String secondFileName;

    static {
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

        try {
            firstFileName = reader.readLine();
            secondFileName = reader.readLine();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    //add your code here - добавьте код тут

    public static void main(String[] args) throws InterruptedException {
        systemOutPrintln(firstFileName);
        systemOutPrintln(secondFileName);
    }

    public static void systemOutPrintln(String fileName) throws InterruptedException {
        ReadFileInterface f = new ReadFileThread();
        f.setFileName(fileName);
        f.start();
        f.join();//add your code here - добавьте код тут
        System.out.println(f.getFileContent());
    }

    public interface ReadFileInterface {

        void setFileName(String fullFileName);

        String getFileContent();

        void join() throws InterruptedException;

        void start();
    }

    public static class ReadFileThread extends Thread implements ReadFileInterface {

        String name;
        String readName="";

        @Override
        public void setFileName(String fullFileName) {
            name = fullFileName;
        }

        @Override
        public String getFileContent() {
            if (readName != null)
                return this.readName;
            else
                return "";
        }

        @Override
        public void run() {

            try {

                Scanner reader = new Scanner(Paths.get(name));
                while (reader.hasNext()) {
                    readName += reader.next() + " ";

                }
                reader.close();
            } catch (Exception e) {
                e.printStackTrace();

            }
        }
     
    }
}
