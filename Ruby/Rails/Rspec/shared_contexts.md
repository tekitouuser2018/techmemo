コード例：
```ruby
RSpec.shared_context "user with tasks and projects" do
  let!(:user) { FactoryBot.create(:user) }
  let!(:project1) { FactoryBot.create(:project, user: user) }
  let!(:project2) { FactoryBot.create(:project, user: user) }
  let!(:task1) { FactoryBot.create(:task, user: user, project: project1) }
  let!(:task2) { FactoryBot.create(:task, user: user, project: project2) }
end

RSpec.describe Task, type: :model do
  describe "validations" do
    include_context "user with tasks and projects"

    it "is invalid without a title" do
      task = FactoryBot.build(:task, title: nil, user: user, project: project1)
      expect(task).not_to be_valid
    end

    it "is invalid without a user" do
      task = FactoryBot.build(:task, user: nil, project: project1)
      expect(task).not_to be_valid
    end

    it "is invalid without a project" do
      task = FactoryBot.build(:task, title: "Test", user: user, project: nil)
      expect(task).not_to be_valid
    end

    it "is valid with a title, user, and project" do
      task = FactoryBot.build(:task, title: "Test", user: user, project: project1)
      expect(task).to be_valid
    end
  end

  describe ".for_user" do
    include_context "user with tasks and projects"

    it "returns all tasks for a user" do
      expect(Task.for_user(user)).to eq([task1, task2])
    end
  end

  describe ".for_project" do
    include_context "user with tasks and projects"

    it "returns all tasks for a project" do
      expect(Task.for_project(project1)).to eq([task1])
      expect(Task.for_project(project2)).to eq([task2])
    end
  end
end
```

