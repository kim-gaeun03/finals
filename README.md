 <finals>

  # 근처 맛집 공유 프로그램

 < 1. 개요>

- 1.1 목적
청주대 주변의 맛집 정보를 공유하고 관리할 수 있는 프로그램입니다.
사용자는 맛집을 등록하고, 찜하기 기능과 리뷰를 통해 맛집 정보를 편리하게 활용할 수 있습니다.

- 1.2 대상
청주대 학생, 교직원, 또는 청주대 주변 맛집 정보를 쉽게 관리하고 싶은 사람들.



<2. 프로그램의 중요성 및 필요성>

- 2.1 중요성
학교 주변의 맛집 정보를 효율적으로 관리하고, 공유하며, 추천받을 수 있는 시스템은 학생들과 교직원의 생활 편의를 높입니다.

- 2.2 필요성
학생들이 자주 찾는 맛집의 위치, 리뷰, 평점을 확인 가능.
사용자 간의 선호도를 분석하여 맛집을 추천.

<3. 프로그램 수행 절차>
-근처 맛집 공유 프로그램

- 맛집 등록 --> 삭제 기능 추가!
- 맛집 조회
- 찜하기
- 평점과 리뷰
- 데이터 저장 및 종료 

<4. 프로그램 동작 절차>
- 프로그램 시작
  : 저장된 데이터 불러오기
- 맛집 등록
  : 이름, 위치 , 설명 입력.
- 맛집 조회
  : 등록된 맛집 리스트를 키워드 검색 또는 전체 출력
- 찜하기
  : 찜 목록에 맛집 추가 및 출력
- 평점과 리뷰 등록
  : 별점(1~5)과 리뷰 추가.
- 저장 및 종료
  : 모든 데이터를 파일에 저장하고 종료.

/*
 * @2024-12-10 프로그램 주제 설정 [청주대 맛집 추천 프로그램] (세부 설정일부는 GPT의 도움받아 제작)
 * import 작성
 * 프레임 제작
 * 테이블, 버튼 제작
 * 버튼 작동
 * 맛집 등록 기능
 * 리뷰 추가
 * 맛집 클래스
 * new ! 맛집 삭제 기능 추가 
 */




import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;

public class Menu { 
    private JFrame frame;
    private JTable table;
    private DefaultTableModel tableModel;
    private ArrayList<Restaurant> restaurants;

    public Menu() {
        restaurants = new ArrayList<>();
        initialize();
    }

    private void initialize() {
        // 메인 프레임 설정
        frame = new JFrame("맛집 공유 프로그램");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(600, 400);
        frame.setLayout(new BorderLayout());

        // 테이블 설정
        String[] columnNames = {"맛집 이름", "위치", "평점", "리뷰"};
        tableModel = new DefaultTableModel(columnNames, 0);
        table = new JTable(tableModel);
        JScrollPane scrollPane = new JScrollPane(table);

        // 버튼 패널
        JPanel buttonPanel = new JPanel();
        JButton addButton = new JButton("맛집 등록");
        JButton reviewButton = new JButton("평점/리뷰 추가");
        JButton deleteButton = new JButton("맛집 삭제");
        JButton exitButton = new JButton("종료");

        buttonPanel.add(addButton);
        buttonPanel.add(reviewButton);
        buttonPanel.add(deleteButton);
        buttonPanel.add(exitButton);

        
        frame.add(scrollPane, BorderLayout.CENTER);
        frame.add(buttonPanel, BorderLayout.SOUTH);

        // 버튼 작동
        addButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                addRestaurant();
            }
        });

        reviewButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                addReview();
            }
        });

        deleteButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
            	deleteRestaurant();
            }
        });

        exitButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.exit(0);
            }
        });

        frame.setVisible(true);
    }

    // 맛집 등록 
    private void addRestaurant() {
        JTextField nameField = new JTextField();
        JTextField locationField = new JTextField();
        Object[] message = {
                "맛집 이름:", nameField,
                "맛집 위치:", locationField
        };

        int option = JOptionPane.showConfirmDialog(frame, message, "맛집 등록", JOptionPane.OK_CANCEL_OPTION);
        if (option == JOptionPane.OK_OPTION) {
            String name = nameField.getText();
            String location = locationField.getText();
            if (!name.isEmpty() && !location.isEmpty()) {
                restaurants.add(new Restaurant(name, location));
                tableModel.addRow(new Object[]{name, location, 0, "리뷰 없음"});
                JOptionPane.showMessageDialog(frame, "맛집이 등록되었습니다!");
            } else {
                JOptionPane.showMessageDialog(frame, "모든 필드를 입력하세요.");
            }
        }
    }

    // 리뷰 추가
    private void addReview() {
        if (restaurants.isEmpty()) {
            JOptionPane.showMessageDialog(frame, "등록된 맛집이 없습니다.");
            return;
        }

        String[] restaurantNames = restaurants.stream()
                .map(Restaurant::getName)
                .toArray(String[]::new);

        String selectedRestaurant = (String) JOptionPane.showInputDialog(
                frame,
                "리뷰를 추가할 맛집을 선택하세요:",
                "맛집 선택",
                JOptionPane.QUESTION_MESSAGE,
                null,
                restaurantNames,
                restaurantNames[0]);

        if (selectedRestaurant != null) {
            for (Restaurant restaurant : restaurants) {
                if (restaurant.getName().equals(selectedRestaurant)) {
                    JTextField ratingField = new JTextField();
                    JTextField reviewField = new JTextField();
                    Object[] message = {
                            "평점 (0-5):", ratingField,
                            "리뷰:", reviewField
                    };

                    int option = JOptionPane.showConfirmDialog(frame, message, "평점/리뷰 추가", JOptionPane.OK_CANCEL_OPTION);
                    if (option == JOptionPane.OK_OPTION) {
                        try {
                            int rating = Integer.parseInt(ratingField.getText());
                            String review = reviewField.getText();
                            if (rating < 0 || rating > 5) {
                                JOptionPane.showMessageDialog(frame, "평점은 0에서 5 사이로 입력하세요.");
                            } else {
                                restaurant.setRating(rating);
                                restaurant.setReview(review);
                                updateTable();
                                JOptionPane.showMessageDialog(frame, "리뷰가 등록되었습니다!");
                            }
                        } catch (NumberFormatException ex) {
                            JOptionPane.showMessageDialog(frame, "평점은 숫자로 입력하세요.");
                        }
                    }
                    break;
                }
            }
        }
    }

    // 맛집 삭제 
    private void deleteRestaurant() {
        int selectedRow = table.getSelectedRow();
        if (selectedRow == -1) {
            JOptionPane.showMessageDialog(frame, "삭제할 맛집을 선택하세요.");
        } else {
            restaurants.remove(selectedRow);
            tableModel.removeRow(selectedRow);
            JOptionPane.showMessageDialog(frame, "맛집이 삭제되었습니다.");
        }
    }
    
    private void updateTable() {
        tableModel.setRowCount(0); 
        for (Restaurant restaurant : restaurants) {
            tableModel.addRow(new Object[]{
                    restaurant.getName(),
                    restaurant.getLocation(),
                    restaurant.getRating(),
                    restaurant.getReview()
            });
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(Menu::new);
    }
}

// 클래스
class Restaurant {
    private String name;
    private String location;
    private String review;
    private int rating;

    public Restaurant(String name, String location) {
        this.name = name;
        this.location = location;
        this.review = "리뷰 없음";
        this.rating = 0;
    }

    public String getName() {
        return name;
    }

    public String getLocation() {
        return location;
    }

    public String getReview() {
        return review;
    }

    public void setReview(String review) {
        this.review = review;
    }

    public int getRating() {
        return rating;
    }

    public void setRating(int rating) {
        this.rating = rating;
    }
}

   







