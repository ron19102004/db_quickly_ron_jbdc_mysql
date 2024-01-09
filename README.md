VÍ DỤ SỬ DỤNG    


    class Color extends EntityBase<Color> {
        private String name;
        private String value;
    
        public Color() {
            super(0);
        }
    
        public Color(String name, String value, int id) {
            super(id);
        }
    
        @Override
        public Color getOne(ResultSet resultSet) {
            try {
                this.name = resultSet.getString("name");
                this.value = resultSet.getString("value");
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
            return this;
        }
        @Override
        public Map<String, String> getAttributes() {
            Map<String, String> attrs = new HashMap<>();
            attrs.put("name", this.name);
            attrs.put("value", this.value);
            return attrs;
        }
    
        @Override
        public int getId() {
            return this.id;
        }
    
        public void setName(String name) {
            this.name = name;
        }
    
        public void setValue(String value) {
            this.value = value;
        }
    
        @Override
        public String toString() {
            return "Color{" +
                    "name='" + name + '\'' +
                    ", value='" + value + '\'' +
                    '}';
        }
    }
    class ColorService extends Service<Color> {
        public ColorService() {
            super("colors", "name,value,id");
        }
    
        @Override
        public ArrayList<Color> find() throws SQLException {
            ArrayList<Color> colors = new ArrayList<>();
            ResultSet resultSet = this.repository.find();
            while (resultSet.next()) {
                Color color = new Color();
                colors.add(color.getOne(resultSet));
            }
            return colors;
        }
    
        @Override
        public Color findById(int id) throws SQLException {
            ResultSet resultSet = this.repository.findById(id);
            if (resultSet.next()) {
                Color color = new Color();
                return color.getOne(resultSet);
            }
            return null;
        }
    
        @Override
        public boolean update(int id,Map<String, String> attributes) throws SQLException {
            return this.repository.update(id,attributes);
        }
    
        @Override
        public boolean save(Map<String, String> attributes) throws SQLException {
            return this.repository.save(attributes);
        }
    
        @Override
        public boolean delete(int id) throws SQLException {
            return this.repository.delete(id);
        }
    }
    public class Test {
        public static void main(String[] args) throws SQLException {
            DB.config()
                    .host("localhost")
                    .port(3306)
                    .username("root")
                    .dbName("webprj2")
                    .run();
            ColorService colorService = new ColorService();
            //get all
            ArrayList<Color> colors = colorService.find();
            //get by id
            Color color = colorService.findById(1);
            //update
            color.setName("new name");
            color.setValue("value new");
            colorService.update(color.getId(),color.getAttributes());
            //delete
            colorService.delete(color.getId());
            //anymore! you can add new other func
        }
    }
