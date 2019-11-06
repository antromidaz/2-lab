package com.company;
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

import static java.lang.Math.*;

public class MainFrame extends JFrame {
   
    private static final int WIDTH = 1000;
    private static final int HEIGHT = 820;

    private JTextField textFieldX;
    private JTextField textFieldY;
    private JTextField textFieldZ;
    private JTextField memoryTextField1;
    private JTextField memoryTextField2;
    private JTextField memoryTextField3;
    private JTextField resultFieldText;

    private JLabel imageLabel;
    private JLabel labelForMem1;
    private JLabel labelForMem2;
    private JLabel labelForMem3;

    private ButtonGroup radioButtons = new ButtonGroup();
    private ButtonGroup radioMemoryButtons = new ButtonGroup();

    private Box hboxFormulaType = Box.createHorizontalBox();
    private Box hBoxMemoryType = Box.createHorizontalBox();
    private Box hBoxFormulaImage = Box.createHorizontalBox();
    private Box hboxResult = Box.createHorizontalBox();
    private Box hBoxMemoryField = Box.createHorizontalBox();
    private Box hBoxControlButtons = Box.createHorizontalBox();
    private int formulaId = 1;
    private int memoryId = 1;

    private Double mem1 = 0.0;
    private Double mem2 = 0.0;
    private Double mem3 = 0.0;

    public Double calculate1(Double x, Double y, Double z) {
        if (y <= 0) {
            JOptionPane.showMessageDialog(MainFrame.this,
                    "Введите у больше 0 ", "Ошибка Ввода", JOptionPane.WARNING_MESSAGE);
            return 0.0;

        }

        return (sin(log(y) + sin(PI * pow(y, 2))) * pow((pow(x, 2) + sin(z) + pow(E, cos(z))), 1 / 4));
    }

    public Double calculate2(Double x, Double y, Double z) {
        return (pow(cos(pow(E, x)) + log(pow((1 + y), 2)) + sqrt(pow(E, cos(x)) + pow(sin(PI * z), 2)) +
                sqrt(1 / x) + cos(pow(y, 2)), sin(z)));
    }

    private void addRadioButton(String buttonName, final int formulaId) {
        JRadioButton button = new JRadioButton(buttonName);
        button.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent ev) {
                MainFrame.this.formulaId = formulaId;
                if (formulaId == 1) imageLabel.setIcon(new ImageIcon(MainFrame.class.getResource("Formula_1.png")));
                if (formulaId == 2) imageLabel.setIcon(new ImageIcon(MainFrame.class.getResource("Formula_2.png")));
            }
        });
        radioButtons.add(button);
        hboxFormulaType.add(button);
    }

    private void addMemoryRadioButton(String buttonName, final int memoryId) {
        JRadioButton button = new JRadioButton(buttonName);
        button.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent event) {
                MainFrame.this.memoryId = memoryId;
            }
        });
        radioMemoryButtons.add(button);
        hBoxMemoryType.add(button);
    }

    public MainFrame() {
        super("Formula Calculation");
        setSize(WIDTH, HEIGHT);
        Toolkit kit = Toolkit.getDefaultToolkit();
        setLocation((kit.getScreenSize().width - WIDTH) / 2,
                (kit.getScreenSize().height - HEIGHT) / 2);

        hboxFormulaType.add(Box.createHorizontalGlue());
        addRadioButton("Formula 1", 1);
        addRadioButton("Formula 2", 2);
        radioButtons.setSelected(radioButtons.getElements().nextElement().getModel(), true);
        hboxFormulaType.add(Box.createHorizontalGlue());
        hboxFormulaType.setBorder(BorderFactory.createLineBorder(Color.YELLOW));

        hBoxFormulaImage.add(Box.createHorizontalGlue());
        imageLabel = new JLabel(new ImageIcon("Formula_1.png"));
        hBoxFormulaImage.add(imageLabel);
        hBoxFormulaImage.add(Box.createHorizontalGlue());
        hBoxFormulaImage.setBorder(BorderFactory.createLineBorder(Color.BLUE));


        JLabel labelForX = new JLabel("X:");
        textFieldX = new JTextField("0.0", 10);
        textFieldX.setMaximumSize(textFieldX.getPreferredSize());
        JLabel labelForY = new JLabel("Y:");
        textFieldY = new JTextField("0.0", 10);
        textFieldY.setMaximumSize(textFieldY.getPreferredSize());
        JLabel labelForZ = new JLabel("Z:");
        textFieldZ = new JTextField("0.0", 10);
        textFieldZ.setMaximumSize(textFieldZ.getPreferredSize());

        Box hboxVariables = Box.createHorizontalBox();
        hboxVariables.setBorder(BorderFactory.createLineBorder(Color.RED));
        hboxVariables.add(Box.createHorizontalGlue());
        hboxVariables.add(labelForX);
        hboxVariables.add(Box.createHorizontalStrut(10));
        hboxVariables.add(textFieldX);
        hboxVariables.add(Box.createHorizontalStrut(50));
        hboxVariables.add(labelForY);
        hboxVariables.add(Box.createHorizontalStrut(10));
        hboxVariables.add(textFieldY);
        hboxVariables.add(Box.createHorizontalStrut(50));
        hboxVariables.add(labelForZ);
        hboxVariables.add(Box.createHorizontalStrut(10));
        hboxVariables.add(textFieldZ);
        hboxVariables.add(Box.createHorizontalGlue());


        JLabel labelForResult = new JLabel("Результат:");
        resultFieldText = new JTextField("0", 15);
        resultFieldText.setMaximumSize(resultFieldText.getPreferredSize());

        hboxResult.add(Box.createHorizontalGlue());
        hboxResult.add(labelForResult);
        hboxResult.add(Box.createHorizontalStrut(10));
        hboxResult.add(resultFieldText);
        hboxResult.add(Box.createHorizontalGlue());
        hboxResult.setBorder(BorderFactory.createLineBorder(Color.black));


        JButton buttonCalc = new JButton("Вычислить");
        buttonCalc.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent ev) {
                try {
                    Double x = Double.parseDouble(textFieldX.getText());
                    Double y = Double.parseDouble(textFieldY.getText());
                    Double z = Double.parseDouble(textFieldZ.getText());
                    Double result;

                    if (formulaId == 1)
                        result = calculate1(x, y, z);
                    else
                        result = calculate2(x, y, z);
                    resultFieldText.setText(result.toString());
                } catch (NumberFormatException ex) {
                    JOptionPane.showMessageDialog(MainFrame.this, "Ошибка в" +
                                    "формате записи числа с плавающей точкой", "Ошибочный формат числа",
                            JOptionPane.WARNING_MESSAGE);
                }
            }
        });


        hBoxMemoryType.add(Box.createHorizontalGlue());
        addMemoryRadioButton("Memory 1", 1);
        addMemoryRadioButton("Memory 2", 2);
        addMemoryRadioButton("Memory 3", 3);
        radioMemoryButtons.setSelected(radioMemoryButtons.getElements().nextElement().getModel(), true);
        hBoxMemoryType.add(Box.createHorizontalGlue());
        hBoxMemoryType.setBorder(BorderFactory.createLineBorder(Color.RED));


        JButton buttonReset = new JButton("Очистить поля");
        buttonReset.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent ev) {
                textFieldX.setText("0.0");
                textFieldY.setText("0.0");
                textFieldZ.setText("0.0");
                resultFieldText.setText("0.0");
                JOptionPane.showMessageDialog(MainFrame.this, "Поля отчищены!", "Очистка", JOptionPane.PLAIN_MESSAGE);
            }
        });

        JButton buttonMC = new JButton("MC");
        buttonMC.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent event) {

                if (memoryId == 1) {
                    mem1 = (double) 0;
                    memoryTextField1.setText("0.0");
                }
                if (memoryId == 2) {
                    mem2 = (double) 0;
                    memoryTextField2.setText("0.0");
                }
                if (memoryId == 3) {
                    mem3 = (double) 0;
                    memoryTextField3.setText("0.0");
                }

            }
        });

        memoryTextField1 = new JTextField("0.0", 15);
        memoryTextField1.setMaximumSize(memoryTextField1.getPreferredSize());
        memoryTextField2 = new JTextField("0.0", 15);
        memoryTextField2.setMaximumSize(memoryTextField2.getPreferredSize());
        memoryTextField3 = new JTextField("0.0", 15);
        memoryTextField3.setMaximumSize(memoryTextField3.getPreferredSize());

        labelForMem1 = new JLabel("Mem1:");
        labelForMem2 = new JLabel("Mem2:");
        labelForMem3 = new JLabel("Mem3:");

        hBoxMemoryField.add(Box.createHorizontalGlue());
        hBoxMemoryField.add(labelForMem1);
        hBoxMemoryField.add(Box.createHorizontalStrut(10));
        hBoxMemoryField.add(memoryTextField1);
        hBoxMemoryField.add(Box.createHorizontalStrut(50));
        hBoxMemoryField.add(labelForMem2);
        hBoxMemoryField.add(Box.createHorizontalStrut(10));
        hBoxMemoryField.add(memoryTextField2);
        hBoxMemoryField.add(Box.createHorizontalStrut(50));
        hBoxMemoryField.add(labelForMem3);
        hBoxMemoryField.add(Box.createHorizontalStrut(10));
        hBoxMemoryField.add(memoryTextField3);
        hBoxMemoryField.add(Box.createHorizontalGlue());


        JButton buttonMp = new JButton("M+");
        buttonMp.addActionListener(new ActionListener() {

            @Override
            public void actionPerformed(ActionEvent arg0) {
                try {
                    Double result = Double.parseDouble(resultFieldText.getText());

                    if (memoryId == 1) {
                        mem1 += result;
                        memoryTextField1.setText(mem1.toString());
                    }
                    if (memoryId == 2) {
                        mem2 += result;
                        memoryTextField2.setText(mem2.toString());
                    }
                    if (memoryId == 3) {
                        mem3 += result;
                        memoryTextField3.setText(mem3.toString());
                    }

                } catch (NumberFormatException ex) {
                    JOptionPane.showMessageDialog(MainFrame.this,
                            "Ошибка в формате записи числа с плавающей точкой", "" +
                                    "Ошибочный формат числа", JOptionPane.WARNING_MESSAGE);
                }
            }
        });


        hBoxControlButtons.add(Box.createHorizontalGlue());
        hBoxControlButtons.add(buttonMC);
        hBoxControlButtons.add(Box.createHorizontalStrut(30));
        hBoxControlButtons.add(buttonMp);
        hBoxControlButtons.add(Box.createHorizontalGlue());


        Box hboxButtons;
        hboxButtons = Box.createHorizontalBox();
        hboxButtons.add(Box.createHorizontalGlue());
        hboxButtons.add(buttonCalc);
        hboxButtons.add(Box.createHorizontalStrut(30));
        hboxButtons.add(buttonReset);
        hboxButtons.add(Box.createHorizontalGlue());
        hboxButtons.setBorder(BorderFactory.createLineBorder(Color.GREEN));


        Box contentBox = Box.createVerticalBox();
        contentBox.add(Box.createVerticalGlue());
        contentBox.add(hBoxFormulaImage);
        contentBox.add(hboxFormulaType);
        contentBox.add(hboxVariables);
        contentBox.add(hboxResult);
        contentBox.add(hboxButtons);
        contentBox.add(hBoxMemoryType);
        contentBox.add(hBoxControlButtons);
        contentBox.add(hBoxMemoryField);
        contentBox.add(Box.createVerticalGlue());
        getContentPane().add(contentBox, BorderLayout.CENTER);
    }
    public static void main(String[] args) {

        MainFrame frame = new MainFrame();


        Toolkit kit = Toolkit.getDefaultToolkit();
        Image icon = kit.getImage("src/com/company/icon2.jpeg");

        frame.setIconImage(icon);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}
