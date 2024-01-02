VÍ DỤ SỬ DỤNG    


    class Color {
        public static final String fillable = "name,value,id";
        public static final String tableName = "colors";
        private String name;
        private String value;
        private int id;
    
        public Color() {
        }
    
        public Color(String name, String value, int id) {
            this.name = name;
            this.value = value;
            this.id = id;
        }
    
        public void ColorOne(ResultSet resultSet) throws SQLException {
            while (resultSet.next()) {
                this.name = resultSet.getString("name");
                this.value = resultSet.getString("value");
                this.id = resultSet.getInt("id");
            }
        }
    
        public void ColorMany(ResultSet resultSet) throws SQLException {
            this.name = resultSet.getString("name");
            this.value = resultSet.getString("value");
            this.id = resultSet.getInt("id");
        }
    
        public Map<String, String> attributes() {
            Map<String, String> attributes = new HashMap<>();
            attributes.put("name", this.name);
            attributes.put("value", this.value);
            return attributes;
        }
    
        @Override
        public String toString() {
            return "Color{" +
                    "name='" + name + '\'' +
                    ", value='" + value + '\'' +
                    ", id=" + id +
                    '}';
        }
    
        public void setName(String name) {
            this.name = name;
        }
    
        public void setValue(String value) {
            this.value = value;
        }
        public int getId() {
            return id;
        }
    }
    public class Main {
        public static void main(String[] args) throws SQLException {
            //Cấu hình CSDL MYSQL với JDBC
            RON.config()
               .host("localhost")
               .port(3306)
               .username("root")
               .password("")
               .dbName("webprj2")
               .run();
            //Tạo RonRepository
            RonRepository colorRepository = new RonRepository(Color.tableName, Color.fillable);
            
            //Lấy tất cả color
            ResultSet resultSet = colorRepository.find();
            ArrayList<Color> colors = new ArrayList<>();
            while (resultSet.next()) {
                Color item = new Color();
                item.ColorMany(resultSet);
                colors.add(item);
            }
            System.out.println(colors);

            //Tìm color theo id
            Color color = new Color();
            color.ColorOne(colorRepository.findById(17));
            System.out.println(color);
    
            color.setName("Trắng");
            color.setValue("#fff");
            //Cập nhật đối tượng color
            colorRepository.update(color.getId(),color.attributes()); //câu lệnh trả về giá trị boolean
    
            //Xóa đối tượng color ra khỏi csdl
            colorRepository.delete(color.getId());
            
            Color colorNew = new Color();
            colorNew.setValue("ValueColor");
            colorNew.setName("NameColor");
            //Thêm đối tượng color vào csdl
            colorRepository.save(colorNew.attributes());
        }
    }
