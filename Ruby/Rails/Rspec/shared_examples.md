コード例：

```ruby
RSpec.shared_examples "a commentable model" do |factory_name|
  let(:model) { FactoryBot.create(factory_name) }
  let(:user) { FactoryBot.create(:user) }

  it "can add a comment" do
    comment = FactoryBot.build(:comment, user: user)
    model.comments << comment
    expect(model.comments).to include(comment)
  end

  it "can remove a comment" do
    comment = FactoryBot.create(:comment, user: user)
    model.comments << comment
    model.comments.delete(comment)
    expect(model.comments).not_to include(comment)
  end

  it "can't add a comment without a user" do
    comment = FactoryBot.build(:comment, user: nil)
    expect { model.comments << comment }.to raise_error(ActiveRecord::RecordInvalid)
  end

  it "can't add a comment without a body" do
    comment = FactoryBot.build(:comment, body: nil, user: user)
    expect { model.comments << comment }.to raise_error(ActiveRecord::RecordInvalid)
  end

  it "can't add a comment with a body longer than 100 characters" do
    comment = FactoryBot.build(:comment, body: "a" * 101, user: user)
    expect { model.comments << comment }.to raise_error(ActiveRecord::RecordInvalid)
  end
end

RSpec.describe Post, type: :model do
  describe "commenting" do
    it_behaves_like "a commentable model", :post
  end
end

RSpec.describe Photo, type: :model do
  describe "commenting" do
    it_behaves_like "a commentable model", :photo
  end
end
```
