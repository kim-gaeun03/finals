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

- 맛집 등록
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
 * @2024-12-10 프로그램 주제 설정 [청주대 맛집 추천 프로그램]
 * import 작성
 * 기본 틀 구성
 * 맛집 관리 클래스 제작
 * ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ(GUI를 이용해 만들어 보고 싶은데 어렵고 복잡해서 이 아래는 챗GPT의 도움을 받음.)ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
 * 
 * 
 * 
 * 
 * 
 * 
 * 
 * 
 * 
 * 
 */








import java.io.*;
import java.util.*;

public class GetMenu {

	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		RestaurantManager manager = new RestaurantManager();
		
		manager.loadRestaurants();

  		while (true) {
            		System.out.println("\n--- 근처 맛집 공유 프로그램 ---");
            		System.out.println("[1] 맛집 등록");
            		System.out.println("[2] 맛집 조회");
            		System.out.println("[3] 평점/리뷰 등록");
            		System.out.println("[4] 프로그램 종료");

            		System.out.print("메뉴 선택: ");
            		int choice = scanner.nextInt();
            		scanner.nextLine(); 

            		switch (choice) {
                		case 1:
                    			manager.addRestaurant(scanner);
                    			break;
                		case 2:
                    			manager.viewRestaurants();
                    			break;
                		case 3:
                    			manager.rateAndReview(scanner);
                    			break;
                		case 4:
                    			manager.saveRestaurants();
                    			System.out.println("프로그램을 종료합니다.");
                    			return;
                		default:
                    			System.out.println("올바른 메뉴를 선택해주세요.");
            		}
        	}
    	}
}

// 맛집 관리 클래스

class RestaurantManager {
    private ArrayList<Restaurant> restaurants;

    public RestaurantManager() {
        restaurants = new ArrayList<>();
    }

    public void addRestaurant(Scanner scanner) {
        System.out.print("맛집 이름: ");
        String name = scanner.nextLine();
        System.out.print("맛집 위치: ");
        String location = scanner.nextLine();
        restaurants.add(new Restaurant(name, location));
        System.out.println("맛집이 등록되었습니다!");
    }

    public void viewRestaurants() {
        if (restaurants.isEmpty()) {
            System.out.println("등록된 맛집이 없습니다.");
        } else {
            System.out.println("--- 등록된 맛집 목록 ---");
            for (int i = 0; i < restaurants.size(); i++) {
                System.out.printf("[%d] %s (%s)\n", i + 1,
                        restaurants.get(i).getName(), restaurants.get(i).getLocation());
            }
        }
    }

    public void addReview(Scanner scanner) {
        if (restaurants.isEmpty()) {
            System.out.println("등록된 맛집이 없습니다.");
            return;
        }

        System.out.println("--- 리뷰 작성할 맛집 선택 ---");
        for (int i = 0; i < restaurants.size(); i++) {
            System.out.printf("[%d] %s\n", i + 1, restaurants.get(i).getName());
        }

        System.out.print("맛집 번호: ");
        int choice = scanner.nextInt();
        scanner.nextLine(); // 개행 문자 소비

        if (choice < 1 || choice > restaurants.size()) {
            System.out.println("올바른 번호를 선택하세요.");
            return;
        }

        Restaurant selectedRestaurant = restaurants.get(choice - 1);
        System.out.print("평점 (0-5): ");
        int rating = scanner.nextInt();
        scanner.nextLine(); // 개행 문자 소비
        System.out.print("리뷰: ");
        String review = scanner.nextLine();

        selectedRestaurant.setRating(rating);
        selectedRestaurant.setReview(review);
        System.out.println("리뷰가 등록되었습니다!");
    }
}

ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ이 아래에 GUI용 새로 만들기ㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡㅡ
